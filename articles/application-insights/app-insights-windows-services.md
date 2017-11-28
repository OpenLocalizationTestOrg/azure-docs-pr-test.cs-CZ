---
title: "aaaAzure Application Insights pro Windows server a pracovní role | Microsoft Docs"
description: "Ručně přidejte hello Application Insights SDK tooyour ASP.NET aplikace tooanalyze využití, dostupnosti a výkonu."
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
ms.openlocfilehash: 64643ef637195d10f87fc6020a77169bca66c1f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manually-configure-application-insights-for-net-applications"></a><span data-ttu-id="ab71a-103">Ruční konfigurace služby Application Insights pro aplikace .NET</span><span class="sxs-lookup"><span data-stu-id="ab71a-103">Manually configure Application Insights for .NET applications</span></span>

<span data-ttu-id="ab71a-104">Můžete nakonfigurovat [Application Insights](app-insights-overview.md) toomonitor celou řadu aplikací nebo aplikační role, komponenty nebo mikroslužeb.</span><span class="sxs-lookup"><span data-stu-id="ab71a-104">You can configure [Application Insights](app-insights-overview.md) toomonitor a wide variety of applications or application roles, components, or microservices.</span></span> <span data-ttu-id="ab71a-105">Pro webové aplikace a služby nabízí Visual Studio [konfiguraci v jednom kroku](app-insights-asp-net.md).</span><span class="sxs-lookup"><span data-stu-id="ab71a-105">For web apps and services, Visual Studio offers [one-step configuration](app-insights-asp-net.md).</span></span> <span data-ttu-id="ab71a-106">V případě jiných typů aplikací .NET, například u rolí back-end serveru nebo u desktopových aplikací, můžete nakonfigurovat službu Application Insights ručně.</span><span class="sxs-lookup"><span data-stu-id="ab71a-106">For other types of .NET application, such as backend server roles or desktop applications, you can configure Application Insights manually.</span></span>

![Příklady tabulek sledování výkonu](./media/app-insights-windows-services/10-perf.png)

#### <a name="before-you-start"></a><span data-ttu-id="ab71a-108">Než začnete</span><span class="sxs-lookup"><span data-stu-id="ab71a-108">Before you start</span></span>

<span data-ttu-id="ab71a-109">Budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="ab71a-109">You need:</span></span>

* <span data-ttu-id="ab71a-110">Předplatné příliš[Microsoft Azure](http://azure.com).</span><span class="sxs-lookup"><span data-stu-id="ab71a-110">A subscription too[Microsoft Azure](http://azure.com).</span></span> <span data-ttu-id="ab71a-111">Pokud váš tým nebo společnost má předplatné Azure, vlastník hello vás může přidat tooit, pomocí vaší [účtu Microsoft](http://live.com).</span><span class="sxs-lookup"><span data-stu-id="ab71a-111">If your team or organization has an Azure subscription, hello owner can add you tooit, using your [Microsoft account](http://live.com).</span></span>
* <span data-ttu-id="ab71a-112">Visual Studio 2013 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ab71a-112">Visual Studio 2013 or later.</span></span>

## <span data-ttu-id="ab71a-113"><a name="add"></a>1. Volba prostředku služby Application Insights</span><span class="sxs-lookup"><span data-stu-id="ab71a-113"><a name="add"></a>1. Choose an Application Insights resource</span></span>

<span data-ttu-id="ab71a-114">Hello prostředků je, kde je vaše data shromažďovat a zobrazí v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ab71a-114">hello 'resource' is where your data is collected and displayed in hello Azure portal.</span></span> <span data-ttu-id="ab71a-115">Jestli potřebujete toodecide toocreate novou, nebo sdílet některého ze stávajících.</span><span class="sxs-lookup"><span data-stu-id="ab71a-115">You need toodecide whether toocreate a new one, or share an existing one.</span></span>

### <a name="part-of-a-larger-app-use-existing-resource"></a><span data-ttu-id="ab71a-116">Součást větší aplikace – použití existujícího prostředku</span><span class="sxs-lookup"><span data-stu-id="ab71a-116">Part of a larger app: Use existing resource</span></span>

<span data-ttu-id="ab71a-117">Pokud vaše webová aplikace má několik komponenty, například front-endové webové aplikace a jeden nebo více back endové služby - pak by měl odeslání telemetrie z všechny součásti toohello hello stejného zdroje.</span><span class="sxs-lookup"><span data-stu-id="ab71a-117">If your web application has several components - for example, a front-end web app and one or more back-end services - then you should send telemetry from all hello components toohello same resource.</span></span> <span data-ttu-id="ab71a-118">Tím je povolit toobe zobrazí na jednu mapu aplikace a bylo možné tootrace žádost od tooanother jedna součást.</span><span class="sxs-lookup"><span data-stu-id="ab71a-118">This will enable them toobe displayed on a single Application Map, and make it possible tootrace a request from one component tooanother.</span></span>

<span data-ttu-id="ab71a-119">Ano, pokud jste již monitorování jiných součástí této aplikace, pak stačí použít hello stejné prostředků.</span><span class="sxs-lookup"><span data-stu-id="ab71a-119">So, if you're already monitoring other components of this app, then just use hello same resource.</span></span>

<span data-ttu-id="ab71a-120">Otevření prostředku hello v hello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="ab71a-120">Open hello resource in hello [Azure portal](https://portal.azure.com/).</span></span> 

### <a name="self-contained-app-create-a-new-resource"></a><span data-ttu-id="ab71a-121">Samostatná aplikace – vytvoření nového prostředku</span><span class="sxs-lookup"><span data-stu-id="ab71a-121">Self-contained app: Create a new resource</span></span>

<span data-ttu-id="ab71a-122">Pokud je nové aplikace hello nesouvisejícími tooother aplikace, měl by mít svůj vlastní prostředek.</span><span class="sxs-lookup"><span data-stu-id="ab71a-122">If hello new app is unrelated tooother applications, then it should have its own resource.</span></span>

<span data-ttu-id="ab71a-123">Přihlaste se toohello [portál Azure](https://portal.azure.com/)a vytvořte nový prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="ab71a-123">Sign in toohello [Azure portal](https://portal.azure.com/), and create a new Application Insights resource.</span></span> <span data-ttu-id="ab71a-124">Vyberte jako typ aplikace hello ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ab71a-124">Choose ASP.NET as hello application type.</span></span>

![Klikněte na tlačítko Nový, Application Insights](./media/app-insights-windows-services/01-new-asp.png)

<span data-ttu-id="ab71a-126">Hello volba typu aplikace nastaví výchozí obsah oken prostředků hello hello.</span><span class="sxs-lookup"><span data-stu-id="ab71a-126">hello choice of application type sets hello default content of hello resource blades.</span></span>

## <a name="2-copy-hello-instrumentation-key"></a><span data-ttu-id="ab71a-127">2. Zkopírujte hello klíč instrumentace</span><span class="sxs-lookup"><span data-stu-id="ab71a-127">2. Copy hello Instrumentation Key</span></span>
<span data-ttu-id="ab71a-128">Hello klíč identifikuje prostředek hello.</span><span class="sxs-lookup"><span data-stu-id="ab71a-128">hello key identifies hello resource.</span></span> <span data-ttu-id="ab71a-129">Nainstalujte ho brzy v hello SDK, v pořadí toodirect data toohello prostředku.</span><span class="sxs-lookup"><span data-stu-id="ab71a-129">You'll install it soon in hello SDK, in order toodirect data toohello resource.</span></span>

![Klikněte na tlačítko Vlastnosti, vyberte klíč hello a stiskněte ctrl + C](./media/app-insights-windows-services/02-props-asp.png)

## <span data-ttu-id="ab71a-131"><a name="sdk"></a>3. Instalovat balíček Application Insights hello v aplikaci</span><span class="sxs-lookup"><span data-stu-id="ab71a-131"><a name="sdk"></a>3. Install hello Application Insights package in your application</span></span>
<span data-ttu-id="ab71a-132">Instalace a konfigurace hello Application Insights se liší v závislosti na platformě hello, kterou právě pracujete na balíčku.</span><span class="sxs-lookup"><span data-stu-id="ab71a-132">Installing and configuring hello Application Insights package varies depending on hello platform you're working on.</span></span> 

1. <span data-ttu-id="ab71a-133">Ve Visual Studiu klikněte pravým tlačítkem na projekt a zvolte **Spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="ab71a-133">In Visual Studio, right-click your project and choose **Manage Nuget Packages**.</span></span>
   
    ![Klikněte pravým tlačítkem na projekt hello a vyberte spravovat balíčky Nuget](./media/app-insights-windows-services/03-nuget.png)
2. <span data-ttu-id="ab71a-135">Instalovat balíček hello Application Insights pro aplikace pro Windows server, "Microsoft.ApplicationInsights.WindowsServer."</span><span class="sxs-lookup"><span data-stu-id="ab71a-135">Install hello Application Insights package for Windows server apps, "Microsoft.ApplicationInsights.WindowsServer."</span></span>
   
    ![Vyhledání Application Insights](./media/app-insights-windows-services/04-ai-nuget.png)
   
    <span data-ttu-id="ab71a-137">*Kterou verzi?*</span><span class="sxs-lookup"><span data-stu-id="ab71a-137">*Which version?*</span></span>

    <span data-ttu-id="ab71a-138">Zkontrolujte **zahrnout předběžné verze** Pokud chcete tootry našich nejnovějších funkcí.</span><span class="sxs-lookup"><span data-stu-id="ab71a-138">Check **Include prerelease** if you want tootry our latest features.</span></span> <span data-ttu-id="ab71a-139">Hello relevantní dokumenty nebo blogy Všimněte si, zda je nutné předprodejní verze.</span><span class="sxs-lookup"><span data-stu-id="ab71a-139">hello relevant documents or blogs note whether you need a prerelease version.</span></span>
    
    <span data-ttu-id="ab71a-140">*Mohu použít jiné balíčky?*</span><span class="sxs-lookup"><span data-stu-id="ab71a-140">*Can I use other packages?*</span></span>
   
    <span data-ttu-id="ab71a-141">Ano.</span><span class="sxs-lookup"><span data-stu-id="ab71a-141">Yes.</span></span> <span data-ttu-id="ab71a-142">Zvolte "Microsoft.ApplicationInsights", pokud chcete pouze toouse hello rozhraní API toosend vlastní telemetrii.</span><span class="sxs-lookup"><span data-stu-id="ab71a-142">Choose "Microsoft.ApplicationInsights" if you only want toouse hello API toosend your own telemetry.</span></span> <span data-ttu-id="ab71a-143">balíček aplikace systému Windows Server Hello zahrnuje hello API plus počet dalších balíčků, jako jsou kolekce čítačů výkonu a monitorování závislostí.</span><span class="sxs-lookup"><span data-stu-id="ab71a-143">hello Windows Server package includes hello API plus a number of other packages such as performance counter collection and dependency monitoring.</span></span> 

### <a name="tooupgrade-toofuture-package-versions"></a><span data-ttu-id="ab71a-144">verze balíčku toofuture tooupgrade</span><span class="sxs-lookup"><span data-stu-id="ab71a-144">tooupgrade toofuture package versions</span></span>
<span data-ttu-id="ab71a-145">Jsme vydání nové verze hello SDK z tootime čas.</span><span class="sxs-lookup"><span data-stu-id="ab71a-145">We release a new version of hello SDK from time tootime.</span></span>

<span data-ttu-id="ab71a-146">tooupgrade tooa [novou verzi balíčku hello](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases/), otevřete znovu Správce balíčků NuGet a filtrujte nainstalované balíčky.</span><span class="sxs-lookup"><span data-stu-id="ab71a-146">tooupgrade tooa [new release of hello package](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases/), open NuGet package manager again and filter on installed packages.</span></span> <span data-ttu-id="ab71a-147">Vyberte **Microsoft.ApplicationInsights.WindowsServer** a zvolte **Upgradovat**.</span><span class="sxs-lookup"><span data-stu-id="ab71a-147">Select **Microsoft.ApplicationInsights.WindowsServer** and choose **Upgrade**.</span></span>

<span data-ttu-id="ab71a-148">Pokud jste provedli jakékoli úpravy tooApplicationInsights.config, uložte jeho kopii před upgradem a následně slučte změny do nové verze hello.</span><span class="sxs-lookup"><span data-stu-id="ab71a-148">If you made any customizations tooApplicationInsights.config, save a copy of it before you upgrade, and afterwards merge your changes into hello new version.</span></span>

## <a name="4-send-telemetry"></a><span data-ttu-id="ab71a-149">4. Odeslání telemetrie</span><span class="sxs-lookup"><span data-stu-id="ab71a-149">4. Send telemetry</span></span>
<span data-ttu-id="ab71a-150">**Pokud jste nainstalovali pouze hello rozhraní API balíčku:**</span><span class="sxs-lookup"><span data-stu-id="ab71a-150">**If you installed only hello API package:**</span></span>

* <span data-ttu-id="ab71a-151">Nastavte klíč instrumentace hello v kódu, například `main()`:</span><span class="sxs-lookup"><span data-stu-id="ab71a-151">Set hello instrumentation key in code, for example in `main()`:</span></span> 
  
    <span data-ttu-id="ab71a-152">`TelemetryConfiguration.Active.InstrumentationKey = "`*váš klíč*`";`</span><span class="sxs-lookup"><span data-stu-id="ab71a-152">`TelemetryConfiguration.Active.InstrumentationKey = "` *your key* `";`</span></span> 
* <span data-ttu-id="ab71a-153">[Psát vlastní telemetrii pomocí rozhraní API hello](app-insights-api-custom-events-metrics.md#ikey).</span><span class="sxs-lookup"><span data-stu-id="ab71a-153">[Write your own telemetry using hello API](app-insights-api-custom-events-metrics.md#ikey).</span></span>

<span data-ttu-id="ab71a-154">**Pokud jste nainstalovali další balíčky Application Insights** , pokud dáváte přednost, můžete klíč instrumentace hello .config souboru tooset hello:</span><span class="sxs-lookup"><span data-stu-id="ab71a-154">**If you installed other Application Insights packages,** you can, if you prefer, use hello .config file tooset hello instrumentation key:</span></span>

* <span data-ttu-id="ab71a-155">Upravte soubor ApplicationInsights.config (který byl přidán hello nainstalovat NuGet).</span><span class="sxs-lookup"><span data-stu-id="ab71a-155">Edit ApplicationInsights.config (which was added by hello NuGet install).</span></span> <span data-ttu-id="ab71a-156">Vložte tuto položku těsně před uzavírací značka hello:</span><span class="sxs-lookup"><span data-stu-id="ab71a-156">Insert this just before hello closing tag:</span></span>
  
    <span data-ttu-id="ab71a-157">`<InstrumentationKey>`*klíč instrumentace hello jste zkopírovali*`</InstrumentationKey>`</span><span class="sxs-lookup"><span data-stu-id="ab71a-157">`<InstrumentationKey>` *hello instrumentation key you copied* `</InstrumentationKey>`</span></span>
* <span data-ttu-id="ab71a-158">Ujistěte se, že jsou příliš nastavena hello vlastnosti souboru ApplicationInsights.config v Průzkumníku řešení**akce sestavení = obsah, kopie tooOutput Directory = kopie**.</span><span class="sxs-lookup"><span data-stu-id="ab71a-158">Make sure that hello properties of ApplicationInsights.config in Solution Explorer are set too**Build Action = Content, Copy tooOutput Directory = Copy**.</span></span>

<span data-ttu-id="ab71a-159">Je užitečné tooset klíč instrumentace hello v kódu, pokud chcete příliš[přepínač hello klíč pro konfigurace různých sestavení](app-insights-separate-resources.md).</span><span class="sxs-lookup"><span data-stu-id="ab71a-159">It's useful tooset hello instrumentation key in code if you want too[switch hello key for different build configurations](app-insights-separate-resources.md).</span></span> <span data-ttu-id="ab71a-160">Pokud jste nastavili hello klíč v kódu, nemáte tooset v hello `.config` souboru.</span><span class="sxs-lookup"><span data-stu-id="ab71a-160">If you set hello key in code, you don't have tooset it in hello `.config` file.</span></span>

## <span data-ttu-id="ab71a-161"><a name="run"></a> Spuštění projektu</span><span class="sxs-lookup"><span data-stu-id="ab71a-161"><a name="run"></a> Run your project</span></span>
<span data-ttu-id="ab71a-162">Použití hello **F5** toorun aplikaci a vyzkoušejte ji: Otevřete různé stránky toogenerate nějaké telemetrie.</span><span class="sxs-lookup"><span data-stu-id="ab71a-162">Use hello **F5** toorun your application and try it out: open different pages toogenerate some telemetry.</span></span>

<span data-ttu-id="ab71a-163">V sadě Visual Studio zobrazí počet hello události, které byly odeslány.</span><span class="sxs-lookup"><span data-stu-id="ab71a-163">In Visual Studio, you'll see a count of hello events that have been sent.</span></span>

![Počet událostí v sadě Visual Studio](./media/app-insights-windows-services/appinsights-09eventcount.png)

## <span data-ttu-id="ab71a-165"><a name="monitor"></a> Zobrazení telemetrických dat</span><span class="sxs-lookup"><span data-stu-id="ab71a-165"><a name="monitor"></a> View your telemetry</span></span>
<span data-ttu-id="ab71a-166">Vrátí toohello [portál Azure](https://portal.azure.com/) a procházet tooyour prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="ab71a-166">Return toohello [Azure portal](https://portal.azure.com/) and browse tooyour Application Insights resource.</span></span>

<span data-ttu-id="ab71a-167">Vyhledejte data v tabulkách přehled hello.</span><span class="sxs-lookup"><span data-stu-id="ab71a-167">Look for data in hello Overview charts.</span></span> <span data-ttu-id="ab71a-168">Na první pohled uvidíte pouze jeden nebo dva body.</span><span class="sxs-lookup"><span data-stu-id="ab71a-168">At first, you'll just see one or two points.</span></span> <span data-ttu-id="ab71a-169">Například:</span><span class="sxs-lookup"><span data-stu-id="ab71a-169">For example:</span></span>

![Klikněte na tlačítko prostřednictvím toomore dat](./media/app-insights-windows-services/12-first-perf.png)

<span data-ttu-id="ab71a-171">Klikněte na tlačítko prostřednictvím jakékoli grafu toosee podrobnější metriky.</span><span class="sxs-lookup"><span data-stu-id="ab71a-171">Click through any chart toosee more detailed metrics.</span></span> [<span data-ttu-id="ab71a-172">Další informace o metrikách.</span><span class="sxs-lookup"><span data-stu-id="ab71a-172">Learn more about metrics.</span></span>](app-insights-web-monitor-performance.md)

### <a name="no-data"></a><span data-ttu-id="ab71a-173">Žádná data?</span><span class="sxs-lookup"><span data-stu-id="ab71a-173">No data?</span></span>
* <span data-ttu-id="ab71a-174">Pomocí aplikace hello, otevřete různé stránky tak, aby ji k vygenerování nějaké telemetrie.</span><span class="sxs-lookup"><span data-stu-id="ab71a-174">Use hello application, opening different pages so that it generates some telemetry.</span></span>
* <span data-ttu-id="ab71a-175">Otevřete hello [vyhledávání](app-insights-diagnostic-search.md) dlaždici toosee jednotlivé události.</span><span class="sxs-lookup"><span data-stu-id="ab71a-175">Open hello [Search](app-insights-diagnostic-search.md) tile, toosee individual events.</span></span> <span data-ttu-id="ab71a-176">Někdy trvá událostem trochu déle, tooget skrz kanály metriky hello.</span><span class="sxs-lookup"><span data-stu-id="ab71a-176">Sometimes it takes events a little while longer tooget through hello metrics pipeline.</span></span>
* <span data-ttu-id="ab71a-177">Počkejte několik sekund a klikněte na tlačítko **Aktualizovat**.</span><span class="sxs-lookup"><span data-stu-id="ab71a-177">Wait a few seconds and click **Refresh**.</span></span> <span data-ttu-id="ab71a-178">Pravidelně grafy sami obnovit, ale můžete je aktualizovat ručně pokud čekáte pro některé data tooshow.</span><span class="sxs-lookup"><span data-stu-id="ab71a-178">Charts refresh themselves periodically, but you can refresh manually if you're waiting for some data tooshow up.</span></span>
* <span data-ttu-id="ab71a-179">Viz [Poradce při potížích](app-insights-troubleshoot-faq.md).</span><span class="sxs-lookup"><span data-stu-id="ab71a-179">See [Troubleshooting](app-insights-troubleshoot-faq.md).</span></span>

## <a name="publish-your-app"></a><span data-ttu-id="ab71a-180">Publikování aplikace</span><span class="sxs-lookup"><span data-stu-id="ab71a-180">Publish your app</span></span>
<span data-ttu-id="ab71a-181">Teď nasadit aplikačního serveru tooyour nebo shromažďování dat tooAzure a sledujte hello.</span><span class="sxs-lookup"><span data-stu-id="ab71a-181">Now deploy your application tooyour server or tooAzure and watch hello data accumulate.</span></span>

![Pomocí sady Visual Studio toopublish vaší aplikace](./media/app-insights-windows-services/15-publish.png)

<span data-ttu-id="ab71a-183">Při spuštění v režimu ladění prochází telemetrie skrz kanálu hello, takže byste měli vidět zobrazení dat během sekund.</span><span class="sxs-lookup"><span data-stu-id="ab71a-183">When you run in debug mode, telemetry is expedited through hello pipeline, so that you should see data appearing within seconds.</span></span> <span data-ttu-id="ab71a-184">Při nasazení aplikace v rámci konfigurace verze se data hromadí pomaleji.</span><span class="sxs-lookup"><span data-stu-id="ab71a-184">When you deploy your app in Release configuration, data accumulates more slowly.</span></span>

### <a name="no-data-after-you-publish-tooyour-server"></a><span data-ttu-id="ab71a-185">Žádná data po publikování tooyour serveru?</span><span class="sxs-lookup"><span data-stu-id="ab71a-185">No data after you publish tooyour server?</span></span>
<span data-ttu-id="ab71a-186">Otevřete v bráně firewall serveru porty pro odchozí provoz.</span><span class="sxs-lookup"><span data-stu-id="ab71a-186">Open ports for outgoing traffic in your server's firewall.</span></span> <span data-ttu-id="ab71a-187">V tématu [tuto stránku](https://docs.microsoft.com/azure/application-insights/app-insights-ip-addresses) hello seznam požadované adresy</span><span class="sxs-lookup"><span data-stu-id="ab71a-187">See [this page](https://docs.microsoft.com/azure/application-insights/app-insights-ip-addresses) for hello list of required addresses</span></span> 

### <a name="trouble-on-your-build-server"></a><span data-ttu-id="ab71a-188">Potíže na vašem serveru sestavení?</span><span class="sxs-lookup"><span data-stu-id="ab71a-188">Trouble on your build server?</span></span>
<span data-ttu-id="ab71a-189">Podívejte se na [tuto položku Řešení potíží](app-insights-asp-net-troubleshoot-no-data.md#NuGetBuild).</span><span class="sxs-lookup"><span data-stu-id="ab71a-189">Please see [this Troubleshooting item](app-insights-asp-net-troubleshoot-no-data.md#NuGetBuild).</span></span>

> [!NOTE]
> <span data-ttu-id="ab71a-190">Pokud vaše aplikace generuje mnoho telemetrie, hello modul adaptivního vzorkování automaticky sníží hello svazek, který je odeslán toohello portál odesláním pouze reprezentativní části události.</span><span class="sxs-lookup"><span data-stu-id="ab71a-190">If your app generates a lot of telemetry, hello adaptive sampling module will automatically reduce hello volume that is sent toohello portal by sending only a representative fraction of events.</span></span> <span data-ttu-id="ab71a-191">Ale události, které jsou související toohello stejném požadavku bude vybrán nebo nevybrány jako skupina, takže mohou procházet mezi souvisejícími událostmi.</span><span class="sxs-lookup"><span data-stu-id="ab71a-191">However, events that are related toohello same request will be selected or deselected as a group, so that you can navigate between related events.</span></span> 
> <span data-ttu-id="ab71a-192">[Další informace o vzorkování](app-insights-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="ab71a-192">[Learn about sampling](app-insights-sampling.md).</span></span>
> 
> 

## <a name="video"></a><span data-ttu-id="ab71a-193">Video</span><span class="sxs-lookup"><span data-stu-id="ab71a-193">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a><span data-ttu-id="ab71a-194">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ab71a-194">Next steps</span></span>
* <span data-ttu-id="ab71a-195">[Přidat další telemetrie](app-insights-asp-net-more.md) tooget hello úplné 360 stupňové zobrazení vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ab71a-195">[Add more telemetry](app-insights-asp-net-more.md) tooget hello full 360-degree view of your application.</span></span>

