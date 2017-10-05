---
title: "Azure Application Insights pro server Windows a role pracovního procesu | Dokumentace Microsoftu"
description: "Ručně přidejte do aplikace ASP.NET sadu SDK Application Insights k analýze využití, dostupnosti a výkonu."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 106ba99b-b57a-43b8-8866-e02f626c8190
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/15/2017
ms.author: bwren
ms.openlocfilehash: 4b9f8c618a69c4c157dafeb7f726aae24efad428
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="manually-configure-application-insights-for-net-applications"></a><span data-ttu-id="1d6a0-103">Ruční konfigurace služby Application Insights pro aplikace .NET</span><span class="sxs-lookup"><span data-stu-id="1d6a0-103">Manually configure Application Insights for .NET applications</span></span>

<span data-ttu-id="1d6a0-104">Službu [Application Insights](app-insights-overview.md) můžete nakonfigurovat tak, aby monitorovala celou řadu aplikací nebo aplikačních rolí, komponent a mikroslužeb.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-104">You can configure [Application Insights](app-insights-overview.md) to monitor a wide variety of applications or application roles, components, or microservices.</span></span> <span data-ttu-id="1d6a0-105">Pro webové aplikace a služby nabízí Visual Studio [konfiguraci v jednom kroku](app-insights-asp-net.md).</span><span class="sxs-lookup"><span data-stu-id="1d6a0-105">For web apps and services, Visual Studio offers [one-step configuration](app-insights-asp-net.md).</span></span> <span data-ttu-id="1d6a0-106">V případě jiných typů aplikací .NET, například u rolí back-end serveru nebo u desktopových aplikací, můžete nakonfigurovat službu Application Insights ručně.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-106">For other types of .NET application, such as backend server roles or desktop applications, you can configure Application Insights manually.</span></span>

![Příklady tabulek sledování výkonu](./media/app-insights-windows-services/10-perf.png)

#### <a name="before-you-start"></a><span data-ttu-id="1d6a0-108">Než začnete</span><span class="sxs-lookup"><span data-stu-id="1d6a0-108">Before you start</span></span>

<span data-ttu-id="1d6a0-109">Budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="1d6a0-109">You need:</span></span>

* <span data-ttu-id="1d6a0-110">Předplatné [Microsoft Azure](http://azure.com).</span><span class="sxs-lookup"><span data-stu-id="1d6a0-110">A subscription to [Microsoft Azure](http://azure.com).</span></span> <span data-ttu-id="1d6a0-111">Pokud váš tým nebo společnost má předplatné Azure, vlastník vás do něj může přidat pomocí vašeho [účtu Microsoft](http://live.com).</span><span class="sxs-lookup"><span data-stu-id="1d6a0-111">If your team or organization has an Azure subscription, the owner can add you to it, using your [Microsoft account](http://live.com).</span></span>
* <span data-ttu-id="1d6a0-112">Visual Studio 2013 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-112">Visual Studio 2013 or later.</span></span>

## <span data-ttu-id="1d6a0-113"><a name="add"></a>1. Volba prostředku služby Application Insights</span><span class="sxs-lookup"><span data-stu-id="1d6a0-113"><a name="add"></a>1. Choose an Application Insights resource</span></span>

<span data-ttu-id="1d6a0-114">Prostředkem je místo na webu Azure Portal, kde jsou shromažďována a zobrazena vaše data.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-114">The 'resource' is where your data is collected and displayed in the Azure portal.</span></span> <span data-ttu-id="1d6a0-115">Musíte se rozhodnout, jestli chcete vytvořit nový nebo sdílet existující prostředek.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-115">You need to decide whether to create a new one, or share an existing one.</span></span>

### <a name="part-of-a-larger-app-use-existing-resource"></a><span data-ttu-id="1d6a0-116">Součást větší aplikace – použití existujícího prostředku</span><span class="sxs-lookup"><span data-stu-id="1d6a0-116">Part of a larger app: Use existing resource</span></span>

<span data-ttu-id="1d6a0-117">Pokud má vaše webová aplikace několik komponent – například front-end webovou aplikaci a jednu nebo více back-end služeb – pak byste měli telemetrická data ze všech komponent odesílat do stejného prostředku.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-117">If your web application has several components - for example, a front-end web app and one or more back-end services - then you should send telemetry from all the components to the same resource.</span></span> <span data-ttu-id="1d6a0-118">To umožní jejich zobrazení v rámci jedné Mapy aplikace a bude možné trasovat požadavky mezi jednotlivými komponentami.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-118">This will enable them to be displayed on a single Application Map, and make it possible to trace a request from one component to another.</span></span>

<span data-ttu-id="1d6a0-119">Pokud už tedy monitorujete jiné komponenty této aplikace, pak použijte stejný prostředek.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-119">So, if you're already monitoring other components of this app, then just use the same resource.</span></span>

<span data-ttu-id="1d6a0-120">Prostředek otevřete na webu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="1d6a0-120">Open the resource in the [Azure portal](https://portal.azure.com/).</span></span> 

### <a name="self-contained-app-create-a-new-resource"></a><span data-ttu-id="1d6a0-121">Samostatná aplikace – vytvoření nového prostředku</span><span class="sxs-lookup"><span data-stu-id="1d6a0-121">Self-contained app: Create a new resource</span></span>

<span data-ttu-id="1d6a0-122">Pokud není nová aplikace vázána k ostatním aplikacím, měla by mít svůj vlastní prostředek.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-122">If the new app is unrelated to other applications, then it should have its own resource.</span></span>

<span data-ttu-id="1d6a0-123">Přihlaste se k webu [Azure Portal](https://portal.azure.com/) a vytvořte nový prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-123">Sign in to the [Azure portal](https://portal.azure.com/), and create a new Application Insights resource.</span></span> <span data-ttu-id="1d6a0-124">Vyberte jako typ aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-124">Choose ASP.NET as the application type.</span></span>

![Klikněte na tlačítko Nový, Application Insights](./media/app-insights-windows-services/01-new-asp.png)

<span data-ttu-id="1d6a0-126">Volba typu aplikace nastaví výchozí obsah oken prostředků.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-126">The choice of application type sets the default content of the resource blades.</span></span>

## <a name="2-copy-the-instrumentation-key"></a><span data-ttu-id="1d6a0-127">2. Zkopírování klíče instrumentace</span><span class="sxs-lookup"><span data-stu-id="1d6a0-127">2. Copy the Instrumentation Key</span></span>
<span data-ttu-id="1d6a0-128">Klíč identifikuje prostředek.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-128">The key identifies the resource.</span></span> <span data-ttu-id="1d6a0-129">Brzy ho nainstalujete v sadě SDK, aby bylo možné směrovat data do prostředku.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-129">You'll install it soon in the SDK, in order to direct data to the resource.</span></span>

![Klikněte na tlačítko Vlastnosti, vyberte klíč a stiskněte klávesy ctrl + C](./media/app-insights-windows-services/02-props-asp.png)

## <span data-ttu-id="1d6a0-131"><a name="sdk"></a>3. Instalace balíčku Application Insights v aplikaci</span><span class="sxs-lookup"><span data-stu-id="1d6a0-131"><a name="sdk"></a>3. Install the Application Insights package in your application</span></span>
<span data-ttu-id="1d6a0-132">Instalace a konfigurace balíčku Application Insights se liší podle platformy, se kterou právě pracujete.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-132">Installing and configuring the Application Insights package varies depending on the platform you're working on.</span></span> 

1. <span data-ttu-id="1d6a0-133">Ve Visual Studiu klikněte pravým tlačítkem na projekt a zvolte **Spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-133">In Visual Studio, right-click your project and choose **Manage Nuget Packages**.</span></span>
   
    ![Kliknutí pravým tlačítkem na projekt a výběr možnosti Spravovat balíčky Nuget](./media/app-insights-windows-services/03-nuget.png)
2. <span data-ttu-id="1d6a0-135">Nainstalujte balíček služby Application Insights pro aplikace Windows Serveru – Microsoft.ApplicationInsights.WindowsServer.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-135">Install the Application Insights package for Windows server apps, "Microsoft.ApplicationInsights.WindowsServer."</span></span>
   
    ![Vyhledání Application Insights](./media/app-insights-windows-services/04-ai-nuget.png)
   
    <span data-ttu-id="1d6a0-137">*Kterou verzi?*</span><span class="sxs-lookup"><span data-stu-id="1d6a0-137">*Which version?*</span></span>

    <span data-ttu-id="1d6a0-138">Pokud chcete vyzkoušet nejnovější funkce, zaškrtněte políčko u možnosti **Zahrnout předběžné verze**.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-138">Check **Include prerelease** if you want to try our latest features.</span></span> <span data-ttu-id="1d6a0-139">Informace o tom, jestli potřebujete předběžnou verzi, najdete v příslušné dokumentaci nebo v blozích.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-139">The relevant documents or blogs note whether you need a prerelease version.</span></span>
    
    <span data-ttu-id="1d6a0-140">*Mohu použít jiné balíčky?*</span><span class="sxs-lookup"><span data-stu-id="1d6a0-140">*Can I use other packages?*</span></span>
   
    <span data-ttu-id="1d6a0-141">Ano.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-141">Yes.</span></span> <span data-ttu-id="1d6a0-142">Pokud chcete rozhraní API používat pouze k odesílání vlastních telemetrických dat, zvolte Microsoft.ApplicationInsights.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-142">Choose "Microsoft.ApplicationInsights" if you only want to use the API to send your own telemetry.</span></span> <span data-ttu-id="1d6a0-143">Balíček Windows Serveru zahrnuje rozhraní API spolu s dalšími balíčky, jako jsou například kolekce čítačů výkonu a monitorování závislostí.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-143">The Windows Server package includes the API plus a number of other packages such as performance counter collection and dependency monitoring.</span></span> 

### <a name="to-upgrade-to-future-package-versions"></a><span data-ttu-id="1d6a0-144">Postup upgradu na budoucí verze balíčku</span><span class="sxs-lookup"><span data-stu-id="1d6a0-144">To upgrade to future package versions</span></span>
<span data-ttu-id="1d6a0-145">Čas od času vydáváme nové verze sady SDK.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-145">We release a new version of the SDK from time to time.</span></span>

<span data-ttu-id="1d6a0-146">K upgradu [novou verzi balíčku](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases/) otevřete znovu Správce balíčků NuGet a filtrujte nainstalované balíčky.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-146">To upgrade to a [new release of the package](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases/), open NuGet package manager again and filter on installed packages.</span></span> <span data-ttu-id="1d6a0-147">Vyberte **Microsoft.ApplicationInsights.WindowsServer** a zvolte **Upgradovat**.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-147">Select **Microsoft.ApplicationInsights.WindowsServer** and choose **Upgrade**.</span></span>

<span data-ttu-id="1d6a0-148">Pokud jste provedli jakékoli úpravy souboru ApplicationInsights.config, uložte jeho kopii před upgradem a následně slučte změny do nové verze.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-148">If you made any customizations to ApplicationInsights.config, save a copy of it before you upgrade, and afterwards merge your changes into the new version.</span></span>

## <a name="4-send-telemetry"></a><span data-ttu-id="1d6a0-149">4. Odeslání telemetrie</span><span class="sxs-lookup"><span data-stu-id="1d6a0-149">4. Send telemetry</span></span>
<span data-ttu-id="1d6a0-150">**Pokud jste nainstalovali pouze balíček rozhraní API:**</span><span class="sxs-lookup"><span data-stu-id="1d6a0-150">**If you installed only the API package:**</span></span>

* <span data-ttu-id="1d6a0-151">Nastavte v kódu klíč instrumentace, například v metodě `main()`:</span><span class="sxs-lookup"><span data-stu-id="1d6a0-151">Set the instrumentation key in code, for example in `main()`:</span></span> 
  
    <span data-ttu-id="1d6a0-152">`TelemetryConfiguration.Active.InstrumentationKey = "` *váš klíč* `";`</span><span class="sxs-lookup"><span data-stu-id="1d6a0-152">`TelemetryConfiguration.Active.InstrumentationKey = "` *your key* `";`</span></span> 
* <span data-ttu-id="1d6a0-153">[Napište si vlastní telemetrii pomocí rozhraní API](app-insights-api-custom-events-metrics.md#ikey).</span><span class="sxs-lookup"><span data-stu-id="1d6a0-153">[Write your own telemetry using the API](app-insights-api-custom-events-metrics.md#ikey).</span></span>

<span data-ttu-id="1d6a0-154">**Pokud jste nainstalovali jiné balíčky Application Insights**, můžete k nastavení klíče instrumentace použít soubor .config, pokud tomu dáváte přednost:</span><span class="sxs-lookup"><span data-stu-id="1d6a0-154">**If you installed other Application Insights packages,** you can, if you prefer, use the .config file to set the instrumentation key:</span></span>

* <span data-ttu-id="1d6a0-155">Upravit soubor ApplicationInsights.config (který byl nainstalován nástrojem NuGet).</span><span class="sxs-lookup"><span data-stu-id="1d6a0-155">Edit ApplicationInsights.config (which was added by the NuGet install).</span></span> <span data-ttu-id="1d6a0-156">Vložte tuto položku těsně před uzavírací značku:</span><span class="sxs-lookup"><span data-stu-id="1d6a0-156">Insert this just before the closing tag:</span></span>
  
    <span data-ttu-id="1d6a0-157">`<InstrumentationKey>` *zkopírovaný klíč instrumentace* `</InstrumentationKey>`</span><span class="sxs-lookup"><span data-stu-id="1d6a0-157">`<InstrumentationKey>` *the instrumentation key you copied* `</InstrumentationKey>`</span></span>
* <span data-ttu-id="1d6a0-158">Ujistěte se, že jsou vlastnosti souboru ApplicationInsights.config v Průzkumníku řešení nastavené na: **Build Action = Content, Copy to Output Directory = Copy**.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-158">Make sure that the properties of ApplicationInsights.config in Solution Explorer are set to **Build Action = Content, Copy to Output Directory = Copy**.</span></span>

<span data-ttu-id="1d6a0-159">Nastavení klíče instrumentace v kódu je užitečné v případě, že chcete [přepínat mezi klíči pro různé konfigurace sestavení](app-insights-separate-resources.md).</span><span class="sxs-lookup"><span data-stu-id="1d6a0-159">It's useful to set the instrumentation key in code if you want to [switch the key for different build configurations](app-insights-separate-resources.md).</span></span> <span data-ttu-id="1d6a0-160">Pokud klíč nastavíte v kódu, nemusíte ho nastavovat v souboru `.config`.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-160">If you set the key in code, you don't have to set it in the `.config` file.</span></span>

## <span data-ttu-id="1d6a0-161"><a name="run"></a> Spuštění projektu</span><span class="sxs-lookup"><span data-stu-id="1d6a0-161"><a name="run"></a> Run your project</span></span>
<span data-ttu-id="1d6a0-162">Použijte **F5** ke spuštění aplikace a vyzkoušejte: otevření různých stránek k vygenerování nějaké telemetrie.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-162">Use the **F5** to run your application and try it out: open different pages to generate some telemetry.</span></span>

<span data-ttu-id="1d6a0-163">V sadě Visual Studio zobrazí počet událostí, které byly odeslány.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-163">In Visual Studio, you'll see a count of the events that have been sent.</span></span>

![Počet událostí v sadě Visual Studio](./media/app-insights-windows-services/appinsights-09eventcount.png)

## <span data-ttu-id="1d6a0-165"><a name="monitor"></a> Zobrazení telemetrických dat</span><span class="sxs-lookup"><span data-stu-id="1d6a0-165"><a name="monitor"></a> View your telemetry</span></span>
<span data-ttu-id="1d6a0-166">Přihlaste se na portál [Azure Portal](https://portal.azure.com/) a vyhledejte prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-166">Return to the [Azure portal](https://portal.azure.com/) and browse to your Application Insights resource.</span></span>

<span data-ttu-id="1d6a0-167">Vyhledejte data v tabulkách Přehled.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-167">Look for data in the Overview charts.</span></span> <span data-ttu-id="1d6a0-168">Na první pohled uvidíte pouze jeden nebo dva body.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-168">At first, you'll just see one or two points.</span></span> <span data-ttu-id="1d6a0-169">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1d6a0-169">For example:</span></span>

![Proklikejte se k dalším datům](./media/app-insights-windows-services/12-first-perf.png)

<span data-ttu-id="1d6a0-171">Proklikejte se prostřednictvím jakékoli grafu pro zobrazení podrobnějších metrik.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-171">Click through any chart to see more detailed metrics.</span></span> [<span data-ttu-id="1d6a0-172">Další informace o metrikách.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-172">Learn more about metrics.</span></span>](app-insights-web-monitor-performance.md)

### <a name="no-data"></a><span data-ttu-id="1d6a0-173">Žádná data?</span><span class="sxs-lookup"><span data-stu-id="1d6a0-173">No data?</span></span>
* <span data-ttu-id="1d6a0-174">Použijte aplikaci a otevřete různé stránky tak, aby došlo k vygenerování nějaké telemetrie.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-174">Use the application, opening different pages so that it generates some telemetry.</span></span>
* <span data-ttu-id="1d6a0-175">Otevřete dlaždici [Vyhledávání](app-insights-diagnostic-search.md) a zobrazte jednotlivé události.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-175">Open the [Search](app-insights-diagnostic-search.md) tile, to see individual events.</span></span> <span data-ttu-id="1d6a0-176">Někdy trvá událostem trochu déle, než se dostanou skrz kanály metriky.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-176">Sometimes it takes events a little while longer to get through the metrics pipeline.</span></span>
* <span data-ttu-id="1d6a0-177">Počkejte několik sekund a klikněte na tlačítko **Aktualizovat**.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-177">Wait a few seconds and click **Refresh**.</span></span> <span data-ttu-id="1d6a0-178">Grafy se pravidelně samy aktualizují, ale můžete je aktualizovat ručně, pokud čekáte na zobrazení některých dat.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-178">Charts refresh themselves periodically, but you can refresh manually if you're waiting for some data to show up.</span></span>
* <span data-ttu-id="1d6a0-179">Viz [Poradce při potížích](app-insights-troubleshoot-faq.md).</span><span class="sxs-lookup"><span data-stu-id="1d6a0-179">See [Troubleshooting](app-insights-troubleshoot-faq.md).</span></span>

## <a name="publish-your-app"></a><span data-ttu-id="1d6a0-180">Publikování aplikace</span><span class="sxs-lookup"><span data-stu-id="1d6a0-180">Publish your app</span></span>
<span data-ttu-id="1d6a0-181">Nyní nasaďte aplikaci na server nebo do Azure a sledujte shromažďování dat.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-181">Now deploy your application to your server or to Azure and watch the data accumulate.</span></span>

![K publikování aplikace použijte sadu Visual Studio](./media/app-insights-windows-services/15-publish.png)

<span data-ttu-id="1d6a0-183">Při spuštění v režimu ladění prochází telemetrie skrz kanál, takže byste měli vidět zobrazení dat během několika sekund.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-183">When you run in debug mode, telemetry is expedited through the pipeline, so that you should see data appearing within seconds.</span></span> <span data-ttu-id="1d6a0-184">Při nasazení aplikace v rámci konfigurace verze se data hromadí pomaleji.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-184">When you deploy your app in Release configuration, data accumulates more slowly.</span></span>

### <a name="no-data-after-you-publish-to-your-server"></a><span data-ttu-id="1d6a0-185">Žádná data po publikování na serveru?</span><span class="sxs-lookup"><span data-stu-id="1d6a0-185">No data after you publish to your server?</span></span>
<span data-ttu-id="1d6a0-186">Otevřete v bráně firewall serveru porty pro odchozí provoz.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-186">Open ports for outgoing traffic in your server's firewall.</span></span> <span data-ttu-id="1d6a0-187">Na [této stránce](https://docs.microsoft.com/azure/application-insights/app-insights-ip-addresses) najdete seznam požadovaných adres.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-187">See [this page](https://docs.microsoft.com/azure/application-insights/app-insights-ip-addresses) for the list of required addresses</span></span> 

### <a name="trouble-on-your-build-server"></a><span data-ttu-id="1d6a0-188">Potíže na vašem serveru sestavení?</span><span class="sxs-lookup"><span data-stu-id="1d6a0-188">Trouble on your build server?</span></span>
<span data-ttu-id="1d6a0-189">Podívejte se na [tuto položku Řešení potíží](app-insights-asp-net-troubleshoot-no-data.md#NuGetBuild).</span><span class="sxs-lookup"><span data-stu-id="1d6a0-189">Please see [this Troubleshooting item](app-insights-asp-net-troubleshoot-no-data.md#NuGetBuild).</span></span>

> [!NOTE]
> <span data-ttu-id="1d6a0-190">Pokud vaše aplikace generuje mnoho telemetrických dat, sníží modul adaptivního vzorkování automaticky objem dat odesílaných na portál tím, že budou odesílány pouze reprezentativní vzorky událostí.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-190">If your app generates a lot of telemetry, the adaptive sampling module will automatically reduce the volume that is sent to the portal by sending only a representative fraction of events.</span></span> <span data-ttu-id="1d6a0-191">Události, které se vztahují ke stejnému požadavku, však budou vybrány nebo nevybrány jako skupina, takže mohou procházet mezi souvisejícími událostmi.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-191">However, events that are related to the same request will be selected or deselected as a group, so that you can navigate between related events.</span></span> 
> <span data-ttu-id="1d6a0-192">[Další informace o vzorkování](app-insights-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="1d6a0-192">[Learn about sampling](app-insights-sampling.md).</span></span>
> 
> 

## <a name="video"></a><span data-ttu-id="1d6a0-193">Video</span><span class="sxs-lookup"><span data-stu-id="1d6a0-193">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a><span data-ttu-id="1d6a0-194">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1d6a0-194">Next steps</span></span>
* <span data-ttu-id="1d6a0-195">[Přidejte více telemetrie](app-insights-asp-net-more.md) a získejte komplexní náhled na svoji aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1d6a0-195">[Add more telemetry](app-insights-asp-net-more.md) to get the full 360-degree view of your application.</span></span>

