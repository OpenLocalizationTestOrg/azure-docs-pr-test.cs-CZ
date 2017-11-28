---
title: "aaaSet až analýzy webové aplikace pro technologii ASP.NET pomocí služby Azure Application Insights | Microsoft Docs"
description: "Nakonfigurujte výkon, dostupnost a analýzy využití pro váš web ASP.NET hostovaný místně nebo v Azure."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: d0eee3c0-b328-448f-8123-f478052751db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/15/2017
ms.author: bwren
ms.openlocfilehash: 61a3cdce68da48bfb9450b1d296acc1535f50a38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-application-insights-for-your-aspnet-website"></a><span data-ttu-id="1a3f0-103">Nastavení Application Insights pro web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1a3f0-103">Set up Application Insights for your ASP.NET website</span></span>

<span data-ttu-id="1a3f0-104">Tento postup nakonfiguruje vaše ASP.NET webové aplikace toosend telemetrie toohello [Azure Application Insights](app-insights-overview.md) služby.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-104">This procedure configures your ASP.NET web app toosend telemetry toohello [Azure Application Insights](app-insights-overview.md) service.</span></span> <span data-ttu-id="1a3f0-105">Funguje pro aplikace ASP.NET, které jsou hostované v serveru služby IIS nebo v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-105">It works for ASP.NET apps that are hosted either in your own IIS server or in hello Cloud.</span></span> <span data-ttu-id="1a3f0-106">Získejte grafy a účinný dotazovací jazyk, který vám pomůže pochopit hello výkonu vaší aplikace a jak uživatelé používají, plus Automatické upozornění na selhání nebo problémy s výkonem.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-106">You get charts and a powerful query language that help you understand hello performance of your app and how people are using it, plus automatic alerts on failures or performance issues.</span></span> <span data-ttu-id="1a3f0-107">Celá řada vývojářů najít tyto funkce skvělé jak jsou, ale můžete také rozšířit a přizpůsobit telemetrie hello, pokud je potřeba.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-107">Many developers find these features great as they are, but you can also extend and customize hello telemetry if you need to.</span></span>

<span data-ttu-id="1a3f0-108">Nastavení je otázkou několika kliknutí v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-108">Setup takes just a few clicks in Visual Studio.</span></span> <span data-ttu-id="1a3f0-109">Máte hello možnost tooavoid poplatky omezením hello svazku telemetrie.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-109">You have hello option tooavoid charges by limiting hello volume of telemetry.</span></span> <span data-ttu-id="1a3f0-110">To vám umožní tooexperiment a ladění nebo toomonitor lokality s není mnoha uživateli.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-110">This allows you tooexperiment and debug, or toomonitor a site with not many users.</span></span> <span data-ttu-id="1a3f0-111">Pokud se rozhodnete chcete toogo dopředu a sledovat vaše pracoviště, je snadné tooraise omezení hello později.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-111">When you decide you want toogo ahead and monitor your production site, it's easy tooraise hello limit later.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="1a3f0-112">Než začnete</span><span class="sxs-lookup"><span data-stu-id="1a3f0-112">Before you start</span></span>
<span data-ttu-id="1a3f0-113">Budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="1a3f0-113">You need:</span></span>

* <span data-ttu-id="1a3f0-114">Visual Studio 2013 s aktualizací Update 3 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-114">Visual Studio 2013 update 3 or later.</span></span> <span data-ttu-id="1a3f0-115">Později je lepší.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-115">Later is better.</span></span>
* <span data-ttu-id="1a3f0-116">Předplatné příliš[Microsoft Azure](http://azure.com).</span><span class="sxs-lookup"><span data-stu-id="1a3f0-116">A subscription too[Microsoft Azure](http://azure.com).</span></span> <span data-ttu-id="1a3f0-117">Pokud váš tým nebo společnost má předplatné Azure, vlastník hello vás může přidat tooit, pomocí vaší [účtu Microsoft](http://live.com).</span><span class="sxs-lookup"><span data-stu-id="1a3f0-117">If your team or organization has an Azure subscription, hello owner can add you tooit, by using your [Microsoft account](http://live.com).</span></span>

<span data-ttu-id="1a3f0-118">Pokud vás zajímá, existují toolook alternativní témata na:</span><span class="sxs-lookup"><span data-stu-id="1a3f0-118">There are alternative topics toolook at if you are interested in:</span></span>

* [<span data-ttu-id="1a3f0-119">Instrumentace webové aplikace za běhu</span><span class="sxs-lookup"><span data-stu-id="1a3f0-119">Instrumenting a web app at runtime</span></span>](app-insights-monitor-performance-live-website-now.md)
* [<span data-ttu-id="1a3f0-120">Azure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="1a3f0-120">Azure Cloud Services</span></span>](app-insights-cloudservices.md)

## <span data-ttu-id="1a3f0-121"><a name="ide"></a>Krok 1: Přidání hello Application Insights SDK</span><span class="sxs-lookup"><span data-stu-id="1a3f0-121"><a name="ide"></a> Step 1: Add hello Application Insights SDK</span></span>

<span data-ttu-id="1a3f0-122">Klikněte pravým tlačítkem myši na projekt webové aplikace v Průzkumníku řešení a vyberte postupně **Přidat** > **Telemetrie Application Insights...** nebo **Konfigurovat Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-122">Right-click your web app project in Solution Explorer, and choose **Add** > **Application Insights Telemetry...** or **Configure Application Insights**.</span></span>

![Snímek obrazovky Průzkumníka řešení se zvýrazněnou možností Přidat telemetrii Application Insights](./media/app-insights-asp-net/appinsights-03-addExisting.png)

<span data-ttu-id="1a3f0-124">(V sadě Visual Studio 2015 se také možnost tooadd Application Insights v dialogovém okně Nový projekt hello.)</span><span class="sxs-lookup"><span data-stu-id="1a3f0-124">(In Visual Studio 2015, there's also an option tooadd Application Insights in hello New Project dialog.)</span></span>

<span data-ttu-id="1a3f0-125">Pokračujte – stránka konfigurace toohello Application Insights:</span><span class="sxs-lookup"><span data-stu-id="1a3f0-125">Continue toohello Application Insights configuration page:</span></span>

![Snímek obrazovky stránky registrace vaší aplikace v Application Insights](./media/app-insights-asp-net/visual-studio-register-dialog.png)

<span data-ttu-id="1a3f0-127">**a.**</span><span class="sxs-lookup"><span data-stu-id="1a3f0-127">**a.**</span></span> <span data-ttu-id="1a3f0-128">Vyberte hello účet a předplatné, že používáte tooaccess Azure.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-128">Select hello account and subscription that you use tooaccess Azure.</span></span>

<span data-ttu-id="1a3f0-129">**b.**</span><span class="sxs-lookup"><span data-stu-id="1a3f0-129">**b.**</span></span> <span data-ttu-id="1a3f0-130">Vyberte prostředek hello v Azure, kam chcete toosee hello data z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-130">Select hello resource in Azure where you want toosee hello data from your app.</span></span> <span data-ttu-id="1a3f0-131">Obvyklý postup:</span><span class="sxs-lookup"><span data-stu-id="1a3f0-131">Usually:</span></span>

* <span data-ttu-id="1a3f0-132">Použijte [jeden prostředek pro různé komponenty](app-insights-monitor-multi-role-apps.md) jedné aplikace.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-132">Use a [single resource for different components](app-insights-monitor-multi-role-apps.md) of a single application.</span></span> 
* <span data-ttu-id="1a3f0-133">Vytvořte samostatné prostředky pro nesouvisející aplikace.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-133">Create separate resources for unrelated applications.</span></span>
 
<span data-ttu-id="1a3f0-134">Pokud chcete tooset hello prostředků skupiny nebo hello umístění, kde jsou uložené vaše data, klikněte na tlačítko **nakonfigurovat nastavení**.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-134">If you want tooset hello resource group or hello location where your data is stored, click **Configure settings**.</span></span> <span data-ttu-id="1a3f0-135">Skupiny prostředků jsou použité toocontrol toodata přístup.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-135">Resource groups are used toocontrol access toodata.</span></span> <span data-ttu-id="1a3f0-136">Například pokud máte několik aplikací, které tvoří část hello, stejný systém, můžete ukládat svoje data Application Insights do hello stejnou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-136">For example, if you have several apps that form part of hello same system, you might put their Application Insights data in hello same resource group.</span></span>

<span data-ttu-id="1a3f0-137">**c.**</span><span class="sxs-lookup"><span data-stu-id="1a3f0-137">**c.**</span></span> <span data-ttu-id="1a3f0-138">Nastavte cap při hello volné dat svazku limitu tooavoid poplatky.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-138">Set a cap at hello free data volume limit, tooavoid charges.</span></span> <span data-ttu-id="1a3f0-139">Application Insights je uvolněte tooa určité svazku telemetrie.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-139">Application Insights is free up tooa certain volume of telemetry.</span></span> <span data-ttu-id="1a3f0-140">Po vytvoření hello prostředků, můžete změnit výběr portálu hello otevřením **funkce + ceny** > **Správa svazku dat** > **denní svazek cap**.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-140">After hello resource is created, you can change your selection in hello portal by opening  **Features + pricing** > **Data volume management** > **Daily volume cap**.</span></span>

<span data-ttu-id="1a3f0-141">**d.**</span><span class="sxs-lookup"><span data-stu-id="1a3f0-141">**d.**</span></span> <span data-ttu-id="1a3f0-142">Klikněte na tlačítko **zaregistrovat** toogo dopředu a konfigurace Application Insights pro webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-142">Click **Register** toogo ahead and configure Application Insights for your web app.</span></span> <span data-ttu-id="1a3f0-143">Telemetrie zašle toohello [portál Azure](https://portal.azure.com), během ladění a po publikování aplikace.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-143">Telemetry will be sent toohello [Azure portal](https://portal.azure.com), both during debugging and after you have published your app.</span></span>

<span data-ttu-id="1a3f0-144">**e.**</span><span class="sxs-lookup"><span data-stu-id="1a3f0-144">**e.**</span></span> <span data-ttu-id="1a3f0-145">Pokud nechcete, aby toosend telemetrie toohello portál při ladění, stačí přidat aplikaci tooyour hello Application Insights SDK ale není nakonfigurovat prostředek hello portálu.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-145">If you don't want toosend telemetry toohello portal while you're debugging, just add hello Application Insights SDK tooyour app but don't configure a resource in hello portal.</span></span> <span data-ttu-id="1a3f0-146">Při ladění, bude se mít toosee telemetrie v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-146">You will be able toosee telemetry in Visual Studio while you are debugging.</span></span> <span data-ttu-id="1a3f0-147">Později, můžete se vrátit toothis stránku konfigurace, nebo může počkejte po nasazení aplikace a [přepínač na telemetrie v době běhu](app-insights-monitor-performance-live-website-now.md).</span><span class="sxs-lookup"><span data-stu-id="1a3f0-147">Later, you can return toothis configuration page, or you could wait until after you have deployed your app and [switch on telemetry at run time](app-insights-monitor-performance-live-website-now.md).</span></span>


## <span data-ttu-id="1a3f0-148"><a name="run"></a>Krok 2: Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="1a3f0-148"><a name="run"></a> Step 2: Run your app</span></span>
<span data-ttu-id="1a3f0-149">Spusťte aplikaci pomocí F5.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-149">Run your app with F5.</span></span> <span data-ttu-id="1a3f0-150">Otevřete různé stránky toogenerate nějaké telemetrie.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-150">Open different pages toogenerate some telemetry.</span></span>

<span data-ttu-id="1a3f0-151">V sadě Visual Studio zobrazí počet hello události, které byly zaprotokolovány.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-151">In Visual Studio, you see a count of hello events that have been logged.</span></span>

![Snímek obrazovky sady Visual Studio.](./media/app-insights-asp-net/54.png)

## <a name="step-3-see-your-telemetry"></a><span data-ttu-id="1a3f0-154">Krok 3: Zobrazení vašich telemetrických dat</span><span class="sxs-lookup"><span data-stu-id="1a3f0-154">Step 3: See your telemetry</span></span>
<span data-ttu-id="1a3f0-155">Můžete zobrazit telemetrii v sadě Visual Studio nebo na webovém portálu Application Insights hello.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-155">You can see your telemetry either in Visual Studio or in hello Application Insights web portal.</span></span> <span data-ttu-id="1a3f0-156">Hledání telemetrie v sadě Visual Studio toohelp ladění aplikace.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-156">Search telemetry in Visual Studio toohelp you debug your app.</span></span> <span data-ttu-id="1a3f0-157">Sledování výkonu a využití v hello webový portál, pokud je váš systém za provozu.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-157">Monitor performance and usage in hello web portal when your system is live.</span></span> 

### <a name="see-your-telemetry-in-visual-studio"></a><span data-ttu-id="1a3f0-158">Zobrazení telemetrických dat v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1a3f0-158">See your telemetry in Visual Studio</span></span>

<span data-ttu-id="1a3f0-159">V sadě Visual Studio otevřete okno Application Insights hello.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-159">In Visual Studio, open hello Application Insights window.</span></span> <span data-ttu-id="1a3f0-160">Buď klikněte na tlačítko hello **Application Insights** tlačítko, nebo klikněte pravým tlačítkem na projekt v Průzkumníku řešení, vyberte **Application Insights**a pak klikněte na tlačítko **vyhledávání Live Telemetrie**.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-160">Either click hello **Application Insights** button, or right-click your project in Solution Explorer, select **Application Insights**, and then click **Search Live Telemetry**.</span></span>

<span data-ttu-id="1a3f0-161">V okně hledání Visual Studio Application Insights hello najdete v části hello **Data z relaci ladění** zobrazení pro telemetrii vygenerovanou hello vaší aplikace na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-161">In hello Visual Studio Application Insights Search window, see hello **Data from Debug session** view for telemetry generated in hello server side of your app.</span></span> <span data-ttu-id="1a3f0-162">Experimentujte s filtry hello a klikněte na všechny události toosee podrobněji.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-162">Experiment with hello filters, and click any event toosee more detail.</span></span>

![Snímek obrazovky hello Data z relaci ladění zobrazit v okně hello Application Insights.](./media/app-insights-asp-net/55.png)

> [!NOTE]
> <span data-ttu-id="1a3f0-164">Pokud se nezobrazí žádná data, zajistěte, aby hello časový rozsah je správný a klikněte na ikonu hledání hello.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-164">If you don't see any data, make sure hello time range is correct, and click hello Search icon.</span></span>

<span data-ttu-id="1a3f0-165">[Další informace týkající se nástrojů Application Insights v sadě Visual Studio](app-insights-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="1a3f0-165">[Learn more about Application Insights tools in Visual Studio](app-insights-visual-studio.md).</span></span>

<a name="monitor"></a>
### <a name="see-telemetry-in-web-portal"></a><span data-ttu-id="1a3f0-166">Zobrazení telemetrických dat na webovém portálu</span><span class="sxs-lookup"><span data-stu-id="1a3f0-166">See telemetry in web portal</span></span>

<span data-ttu-id="1a3f0-167">Můžete také zjistit telemetrii na webovém portálu Application Insights hello (Pokud jste vybrali pouze hello tooinstall SDK).</span><span class="sxs-lookup"><span data-stu-id="1a3f0-167">You can also see telemetry in hello Application Insights web portal (unless you chose tooinstall only hello SDK).</span></span> <span data-ttu-id="1a3f0-168">Hello portál obsahuje více grafů, analytických nástrojů a zobrazení mezi komponenty než Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-168">hello portal has more charts, analytic tools, and cross-component views than Visual Studio.</span></span> <span data-ttu-id="1a3f0-169">portál Hello také nabízí výstrahy.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-169">hello portal also provides alerts.</span></span>

<span data-ttu-id="1a3f0-170">Otevřete prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-170">Open your Application Insights resource.</span></span> <span data-ttu-id="1a3f0-171">Buď přihlásit toohello [portál Azure](https://portal.azure.com/) najít ho existuje, nebo klikněte pravým tlačítkem na projekt hello v sadě Visual Studio a nechat ji provést uživatele.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-171">Either sign in toohello [Azure portal](https://portal.azure.com/) and find it there, or right-click hello project in Visual Studio, and let it take you there.</span></span>

![Snímek obrazovky aplikace Visual Studio, znázorňující, jak tooopen hello portál Application Insights](./media/app-insights-asp-net/appinsights-04-openPortal.png)

> [!NOTE]
> <span data-ttu-id="1a3f0-173">Pokud dojde k chybě přístup: máte více než jednu sadu přihlašovacích údajů Microsoft a jsou jste přihlášeni s použitím hello nesprávnou sadu?</span><span class="sxs-lookup"><span data-stu-id="1a3f0-173">If you get an access error: Do you have more than one set of Microsoft credentials, and are you signed in with hello wrong set?</span></span> <span data-ttu-id="1a3f0-174">Hello portálu Odhlásit se a znovu přihlásit.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-174">In hello portal, sign out and sign in again.</span></span>

<span data-ttu-id="1a3f0-175">Hello portál ho otevře v zobrazení hello telemetrie z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-175">hello portal opens on a view of hello telemetry from your app.</span></span>

![Snímek obrazovky stránky s přehledem Application Insights](./media/app-insights-asp-net/66.png)

<span data-ttu-id="1a3f0-177">Hello portálu klikněte na všechny dlaždice nebo graf toosee podrobněji.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-177">In hello portal, click any tile or chart toosee more detail.</span></span>

<span data-ttu-id="1a3f0-178">[Další informace o používání Application Insights na portálu Azure hello](app-insights-dashboards.md).</span><span class="sxs-lookup"><span data-stu-id="1a3f0-178">[Learn more about using Application Insights in hello Azure portal](app-insights-dashboards.md).</span></span>

## <a name="step-4-publish-your-app"></a><span data-ttu-id="1a3f0-179">Krok 4: Publikování aplikace</span><span class="sxs-lookup"><span data-stu-id="1a3f0-179">Step 4: Publish your app</span></span>
<span data-ttu-id="1a3f0-180">Publikování aplikací serveru IIS tooyour nebo tooAzure.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-180">Publish your app tooyour IIS server or tooAzure.</span></span> <span data-ttu-id="1a3f0-181">Kukátko [živý datový proud metriky](app-insights-metrics-explorer.md#live-metrics-stream) toomake se všechno běží bez problémů.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-181">Watch [Live Metrics Stream](app-insights-metrics-explorer.md#live-metrics-stream) toomake sure everything is running smoothly.</span></span>

<span data-ttu-id="1a3f0-182">Telemetrie vytvoří hello portálu Application Insights, kde můžete monitorovat metriky, hledání telemetrie a nastavit [řídicí panely](app-insights-dashboards.md).</span><span class="sxs-lookup"><span data-stu-id="1a3f0-182">Your telemetry builds up in hello Application Insights portal, where you can monitor metrics, search your telemetry, and set up [dashboards](app-insights-dashboards.md).</span></span> <span data-ttu-id="1a3f0-183">Můžete také použít hello výkonné [analýzy protokolů dotazu jazyka](https://docs.loganalytics.io/) tooanalyze využití a výkonu nebo toofind konkrétní události.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-183">You can also use hello powerful [Log Analytics query language](https://docs.loganalytics.io/) tooanalyze usage and performance, or toofind specific events.</span></span>

<span data-ttu-id="1a3f0-184">Můžete také pokračovat tooanalyze telemetrie v [Visual Studio](app-insights-visual-studio.md), pomocí nástrojů, jako je vyhledávání diagnostiky a [trendy](app-insights-visual-studio-trends.md).</span><span class="sxs-lookup"><span data-stu-id="1a3f0-184">You can also continue tooanalyze your telemetry in [Visual Studio](app-insights-visual-studio.md), with tools such as diagnostic search and [trends](app-insights-visual-studio-trends.md).</span></span>

> [!NOTE]
> <span data-ttu-id="1a3f0-185">Pokud vaše aplikace odesílá dost telemetrických dat tooapproach hello [omezení](app-insights-pricing.md#limits-summary), automatické [vzorkování](app-insights-sampling.md) přepne.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-185">If your app sends enough telemetry tooapproach hello [throttling limits](app-insights-pricing.md#limits-summary), automatic [sampling](app-insights-sampling.md) switches on.</span></span> <span data-ttu-id="1a3f0-186">Vzorkování snižuje množství hello telemetrická data odesílaná z vaší aplikace, při zachování korelační data k diagnostickým účelům.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-186">Sampling reduces hello quantity of telemetry sent from your app, while preserving correlated data for diagnostic purposes.</span></span>
>
>

## <span data-ttu-id="1a3f0-187"><a name="land"></a> Nyní je vše připraveno.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-187"><a name="land"></a> You're all set</span></span>

<span data-ttu-id="1a3f0-188">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="1a3f0-188">Congratulations!</span></span> <span data-ttu-id="1a3f0-189">Nainstalovaný balíček Application Insights hello ve vaší aplikaci a nakonfigurovat službu Application Insights toohello telemetrie toosend v Azure.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-189">You installed hello Application Insights package in your app, and configured it toosend telemetry toohello Application Insights service on Azure.</span></span>

![Diagram přesunu telemetrie](./media/app-insights-asp-net/01-scheme.png)

<span data-ttu-id="1a3f0-191">je identifikována Hello prostředků Azure, která přijímá telemetrii aplikace *klíč instrumentace*.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-191">hello Azure resource that receives your app's telemetry is identified by an *instrumentation key*.</span></span> <span data-ttu-id="1a3f0-192">Tento klíč najdete v souboru souboru ApplicationInsights.config hello.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-192">You'll find this key in hello ApplicationInsights.config file.</span></span>


## <a name="upgrade-toofuture-sdk-versions"></a><span data-ttu-id="1a3f0-193">Upgrade verze sady SDK toofuture</span><span class="sxs-lookup"><span data-stu-id="1a3f0-193">Upgrade toofuture SDK versions</span></span>
<span data-ttu-id="1a3f0-194">tooupgrade tooa [novou verzi sady hello SDK](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases), otevřete hello **Správce balíčků NuGet** znovu a filtr na nainstalované balíčky.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-194">tooupgrade tooa [new release of hello SDK](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases), open hello **NuGet package manager** again, and filter on installed packages.</span></span> <span data-ttu-id="1a3f0-195">Vyberte **Microsoft.ApplicationInsights.Web** a zvolte **Upgradovat**.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-195">Select **Microsoft.ApplicationInsights.Web**, and choose **Upgrade**.</span></span>

<span data-ttu-id="1a3f0-196">Pokud jste provedli jakékoli úpravy tooApplicationInsights.config, uložte jeho kopii před upgradem.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-196">If you made any customizations tooApplicationInsights.config, save a copy of it before you upgrade.</span></span> <span data-ttu-id="1a3f0-197">Pak sloučit změny do nové verze hello.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-197">Then, merge your changes into hello new version.</span></span>

## <a name="video"></a><span data-ttu-id="1a3f0-198">Video</span><span class="sxs-lookup"><span data-stu-id="1a3f0-198">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a><span data-ttu-id="1a3f0-199">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1a3f0-199">Next steps</span></span>

### <a name="more-telemetry"></a><span data-ttu-id="1a3f0-200">Další telemetrická data</span><span class="sxs-lookup"><span data-stu-id="1a3f0-200">More telemetry</span></span>

* <span data-ttu-id="1a3f0-201">**[Údaje o prohlížečích a o načítání stránek](app-insights-javascript.md)** – Vložte do svých webových stránek fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-201">**[Browser and page load data](app-insights-javascript.md)** - Insert a code snippet in your web pages.</span></span>
* <span data-ttu-id="1a3f0-202">**[Dosažení podrobnějšího monitorování závislostí a výjimek](app-insights-monitor-performance-live-website-now.md)** – Nainstalujte si na server Monitorování stavu.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-202">**[Get more detailed dependency and exception monitoring](app-insights-monitor-performance-live-website-now.md)** - Install Status Monitor on your server.</span></span>
* <span data-ttu-id="1a3f0-203">**[Kód vlastní události](app-insights-api-custom-events-metrics.md)**  toocount, čas nebo měření akce uživatele.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-203">**[Code custom events](app-insights-api-custom-events-metrics.md)** toocount, time, or measure user actions.</span></span>
* <span data-ttu-id="1a3f0-204">**[Získání dat protokolu](app-insights-asp-net-trace-logs.md)** – Zjišťujte korelaci dat protokolu s telemetrickými daty.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-204">**[Get log data](app-insights-asp-net-trace-logs.md)** - Correlate log data with your telemetry.</span></span>

### <a name="analysis"></a><span data-ttu-id="1a3f0-205">Analýza</span><span class="sxs-lookup"><span data-stu-id="1a3f0-205">Analysis</span></span>

* <span data-ttu-id="1a3f0-206">**[Práce s Application Insights v sadě Visual Studio](app-insights-visual-studio.md)**</span><span class="sxs-lookup"><span data-stu-id="1a3f0-206">**[Working with Application Insights in Visual Studio](app-insights-visual-studio.md)**</span></span><br/><span data-ttu-id="1a3f0-207">Obsahuje informace o ladění pomocí telemetrie, diagnostické vyhledávání a procházení prostřednictvím toocode.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-207">Includes information about debugging with telemetry, diagnostic search, and drill through toocode.</span></span>
* <span data-ttu-id="1a3f0-208">**[Práce s Application Insights portál hello](app-insights-dashboards.md)**</span><span class="sxs-lookup"><span data-stu-id="1a3f0-208">**[Working with hello Application Insights portal](app-insights-dashboards.md)**</span></span><br/> <span data-ttu-id="1a3f0-209">Zahrnuje informace o řídicích panelech, výkonných nástrojích pro diagnostiku a analýzy, výstrahách, aktivních mapách závislostí vaší aplikace a exportu telemetrie.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-209">Includes information about dashboards, powerful diagnostic and analytic tools, alerts, a live dependency map of your application, and telemetry export.</span></span>
* <span data-ttu-id="1a3f0-210">**[Analýza](app-insights-analytics-tour.md)**  -hello účinný dotazovací jazyk.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-210">**[Analytics](app-insights-analytics-tour.md)** - hello powerful query language.</span></span>

### <a name="alerts"></a><span data-ttu-id="1a3f0-211">Výstrahy</span><span class="sxs-lookup"><span data-stu-id="1a3f0-211">Alerts</span></span>

* <span data-ttu-id="1a3f0-212">[Testy dostupnosti](app-insights-monitor-web-app-availability.md): vytváření testů toomake, že je váš web viditelná na webu hello.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-212">[Availability tests](app-insights-monitor-web-app-availability.md): Create tests toomake sure your site is visible on hello web.</span></span>
* <span data-ttu-id="1a3f0-213">[Inteligentní diagnostiky](app-insights-proactive-diagnostics.md): tyto testy se spouštějí automaticky, takže není nutné toodo cokoli tooset je nahoru.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-213">[Smart diagnostics](app-insights-proactive-diagnostics.md): These tests run automatically, so you don't have toodo anything tooset them up.</span></span> <span data-ttu-id="1a3f0-214">Upozorní vás, pokud má aplikace nezvykle velký podíl neúspěšných požadavků.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-214">They tell you if your app has an unusual rate of failed requests.</span></span>
* <span data-ttu-id="1a3f0-215">[Metriky výstrahy](app-insights-alerts.md): nastavte tyto toowarn vám, zda metriky protne prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-215">[Metric alerts](app-insights-alerts.md): Set these toowarn you if a metric crosses a threshold.</span></span> <span data-ttu-id="1a3f0-216">Upozornění můžete nastavit u vlastních metrik, které v aplikaci naprogramujete.</span><span class="sxs-lookup"><span data-stu-id="1a3f0-216">You can set them on custom metrics that you code into your app.</span></span>

### <a name="automation"></a><span data-ttu-id="1a3f0-217">Automation</span><span class="sxs-lookup"><span data-stu-id="1a3f0-217">Automation</span></span>

* [<span data-ttu-id="1a3f0-218">Automatizace vytvoření prostředku Application Insights</span><span class="sxs-lookup"><span data-stu-id="1a3f0-218">Automate creating an Application Insights resource</span></span>](app-insights-powershell.md)
