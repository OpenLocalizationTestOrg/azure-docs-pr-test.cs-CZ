---
title: "aaaMonitor za provozu technologie ASP.NET webové aplikace pomocí služby Azure Application Insights | Microsoft Docs"
description: "Monitorování výkonu webu bez opětovného nasazení. Funguje s místně hostovanými webovými aplikacemi v ASP.NET, na virtuálních počítačích nebo v Azure."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/05/2017
ms.author: bwren
ms.openlocfilehash: 0d53f0a59974f40767fae681bafc4f358d1283a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="instrument-web-apps-at-runtime-with-application-insights"></a><span data-ttu-id="3f254-104">Instrumentace webových aplikací za běhu pomocí nástrojů Application Insights</span><span class="sxs-lookup"><span data-stu-id="3f254-104">Instrument web apps at runtime with Application Insights</span></span>


<span data-ttu-id="3f254-105">Můžete instrumentace živou webovou aplikaci pomocí služby Azure Application Insights, bez nutnosti toomodify nebo znovu nasadit vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="3f254-105">You can instrument a live web app with Azure Application Insights, without having toomodify or redeploy your code.</span></span> <span data-ttu-id="3f254-106">Pokud vaše aplikace hostuje místní server služby IIS, nainstalujte Monitorování stavu.</span><span class="sxs-lookup"><span data-stu-id="3f254-106">If your apps are hosted by an on-premises IIS server, install Status Monitor.</span></span> <span data-ttu-id="3f254-107">Pokud už webové aplikace Azure nebo spusťte virtuální počítač Azure, můžete přepnout na Application Insights monitorování z hello Azure ovládací panely.</span><span class="sxs-lookup"><span data-stu-id="3f254-107">If they're Azure web apps or run in an Azure VM, you can switch on Application Insights monitoring from hello Azure control panel.</span></span> <span data-ttu-id="3f254-108">(Existují i samostatné články o instrumentaci [živých webových aplikací J2EE](app-insights-java-live.md) a [Azure Cloud Services](app-insights-cloudservices.md).) Budete potřebovat předplatné [Microsoft Azure](http://azure.com).</span><span class="sxs-lookup"><span data-stu-id="3f254-108">(There are also separate articles about instrumenting [live J2EE web apps](app-insights-java-live.md) and [Azure Cloud Services](app-insights-cloudservices.md).) You need a [Microsoft Azure](http://azure.com) subscription.</span></span>

![ukázkové grafy](./media/app-insights-monitor-performance-live-website-now/10-intro.png)

<span data-ttu-id="3f254-110">Máte možnost volby tří trasy tooapply Application Insights tooyour .NET webových aplikací:</span><span class="sxs-lookup"><span data-stu-id="3f254-110">You have a choice of three routes tooapply Application Insights tooyour .NET web applications:</span></span>

* <span data-ttu-id="3f254-111">**Čas sestavení:** [hello přidat Application Insights SDK] [ greenbrown] tooyour kódu webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="3f254-111">**Build time:** [Add hello Application Insights SDK][greenbrown] tooyour web app code.</span></span>
* <span data-ttu-id="3f254-112">**Čas spuštění:** instrumentaci vaší webové aplikace na serveru hello, jak je popsáno níže, bez znovu sestavit a znovu nasazovat hello kódu.</span><span class="sxs-lookup"><span data-stu-id="3f254-112">**Run time:** Instrument your web app on hello server, as described below, without rebuilding and redeploying hello code.</span></span>
* <span data-ttu-id="3f254-113">**Obě:** sestavení hello SDK do kódu webové aplikace a také použít rozšíření běhu hello.</span><span class="sxs-lookup"><span data-stu-id="3f254-113">**Both:** Build hello SDK into your web app code, and also apply hello run-time extensions.</span></span> <span data-ttu-id="3f254-114">Získat hello nejlepší z obou možností.</span><span class="sxs-lookup"><span data-stu-id="3f254-114">Get hello best of both options.</span></span>

<span data-ttu-id="3f254-115">Tady je rekapitulace toho, co každý způsob přináší:</span><span class="sxs-lookup"><span data-stu-id="3f254-115">Here's a summary of what you get by each route:</span></span>

|  | <span data-ttu-id="3f254-116">Při sestavení</span><span class="sxs-lookup"><span data-stu-id="3f254-116">Build time</span></span> | <span data-ttu-id="3f254-117">Za běhu</span><span class="sxs-lookup"><span data-stu-id="3f254-117">Run time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3f254-118">Požadavky a výjimky</span><span class="sxs-lookup"><span data-stu-id="3f254-118">Requests & exceptions</span></span> |<span data-ttu-id="3f254-119">Ano</span><span class="sxs-lookup"><span data-stu-id="3f254-119">Yes</span></span> |<span data-ttu-id="3f254-120">Ano</span><span class="sxs-lookup"><span data-stu-id="3f254-120">Yes</span></span> |
| [<span data-ttu-id="3f254-121">Podrobnější výjimky</span><span class="sxs-lookup"><span data-stu-id="3f254-121">More detailed exceptions</span></span>](app-insights-asp-net-exceptions.md) | |<span data-ttu-id="3f254-122">Ano</span><span class="sxs-lookup"><span data-stu-id="3f254-122">Yes</span></span> |
| [<span data-ttu-id="3f254-123">Diagnostika závislostí</span><span class="sxs-lookup"><span data-stu-id="3f254-123">Dependency diagnostics</span></span>](app-insights-asp-net-dependencies.md) |<span data-ttu-id="3f254-124">Na platformě .NET 4.6+, ale méně podrobná</span><span class="sxs-lookup"><span data-stu-id="3f254-124">On .NET 4.6+, but less detail</span></span> |<span data-ttu-id="3f254-125">Ano, úplné podrobnosti: kódy výsledků, text příkazu SQL, příkaz HTTP</span><span class="sxs-lookup"><span data-stu-id="3f254-125">Yes, full detail: result codes, SQL command text, HTTP verb</span></span>|
| [<span data-ttu-id="3f254-126">Čítače výkonu systému</span><span class="sxs-lookup"><span data-stu-id="3f254-126">System performance counters</span></span>](app-insights-performance-counters.md) |<span data-ttu-id="3f254-127">Ano</span><span class="sxs-lookup"><span data-stu-id="3f254-127">Yes</span></span> |<span data-ttu-id="3f254-128">Ano</span><span class="sxs-lookup"><span data-stu-id="3f254-128">Yes</span></span> |
| <span data-ttu-id="3f254-129">[Rozhraní API pro vlastní telemetrii][api]</span><span class="sxs-lookup"><span data-stu-id="3f254-129">[API for custom telemetry][api]</span></span> |<span data-ttu-id="3f254-130">Ano</span><span class="sxs-lookup"><span data-stu-id="3f254-130">Yes</span></span> |<span data-ttu-id="3f254-131">Ne</span><span class="sxs-lookup"><span data-stu-id="3f254-131">No</span></span> |
| [<span data-ttu-id="3f254-132">Integrace protokolu trasování</span><span class="sxs-lookup"><span data-stu-id="3f254-132">Trace log integration</span></span>](app-insights-asp-net-trace-logs.md) |<span data-ttu-id="3f254-133">Ano</span><span class="sxs-lookup"><span data-stu-id="3f254-133">Yes</span></span> |<span data-ttu-id="3f254-134">Ne</span><span class="sxs-lookup"><span data-stu-id="3f254-134">No</span></span> |
| [<span data-ttu-id="3f254-135">Zobrazení stránky a uživatelská data</span><span class="sxs-lookup"><span data-stu-id="3f254-135">Page view & user data</span></span>](app-insights-javascript.md) |<span data-ttu-id="3f254-136">Ano</span><span class="sxs-lookup"><span data-stu-id="3f254-136">Yes</span></span> |<span data-ttu-id="3f254-137">Ne</span><span class="sxs-lookup"><span data-stu-id="3f254-137">No</span></span> |
| <span data-ttu-id="3f254-138">Třeba toorebuild kódu</span><span class="sxs-lookup"><span data-stu-id="3f254-138">Need toorebuild code</span></span> |<span data-ttu-id="3f254-139">Ano</span><span class="sxs-lookup"><span data-stu-id="3f254-139">Yes</span></span> | <span data-ttu-id="3f254-140">Ne</span><span class="sxs-lookup"><span data-stu-id="3f254-140">No</span></span> |


## <a name="monitor-a-live-azure-web-app"></a><span data-ttu-id="3f254-141">Monitorování živé webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="3f254-141">Monitor a live Azure web app</span></span>

<span data-ttu-id="3f254-142">Pokud vaše aplikace běží jako k službě Azure web, sem způsobu tooswitch na monitorování:</span><span class="sxs-lookup"><span data-stu-id="3f254-142">If your application is running as an Azure web service, here's how tooswitch on monitoring:</span></span>

* <span data-ttu-id="3f254-143">Vyberte Application Insights v Ovládacích panelech hello aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="3f254-143">Select Application Insights on hello app's control panel in Azure.</span></span>

    ![Nastavení Application Insights pro webovou aplikaci Azure](./media/app-insights-monitor-performance-live-website-now/azure-web-setup.png)
* <span data-ttu-id="3f254-145">Když se otevře stránka souhrnu hello Application Insights, klepněte na odkaz hello hello dolní tooopen hello úplné prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="3f254-145">When hello Application Insights summary page opens, click hello link at hello bottom tooopen hello full Application Insights resource.</span></span>

    ![Klikněte na tlačítko prostřednictvím tooApplication statistiky](./media/app-insights-monitor-performance-live-website-now/azure-web-view-more.png)

<span data-ttu-id="3f254-147">[Monitorování cloudových aplikací a aplikací virtuálních počítačů](app-insights-azure.md).</span><span class="sxs-lookup"><span data-stu-id="3f254-147">[Monitoring Cloud and VM apps](app-insights-azure.md).</span></span>

### <a name="enable-client-side-monitoring-in-azure"></a><span data-ttu-id="3f254-148">Povolení monitorování na straně klienta v Azure</span><span class="sxs-lookup"><span data-stu-id="3f254-148">Enable client-side monitoring in Azure</span></span>

<span data-ttu-id="3f254-149">Pokud jste v Azure povolili nástroje Application Insights, můžete přidat zobrazení stránky a telemetrii uživatelů.</span><span class="sxs-lookup"><span data-stu-id="3f254-149">If you have enabled Application Insights in Azure, you can add page view and user telemetry.</span></span>

1. <span data-ttu-id="3f254-150">Klikněte na Nastavení > Nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="3f254-150">Select Settings > Application Settings</span></span>
2.  <span data-ttu-id="3f254-151">V části Nastavení aplikace přidejte novou dvojici klíče a hodnoty:</span><span class="sxs-lookup"><span data-stu-id="3f254-151">Under App Settings, add a new key value pair:</span></span> 
   
    <span data-ttu-id="3f254-152">Klíč: `APPINSIGHTS_JAVASCRIPT_ENABLED`</span><span class="sxs-lookup"><span data-stu-id="3f254-152">Key: `APPINSIGHTS_JAVASCRIPT_ENABLED`</span></span> 
    
    <span data-ttu-id="3f254-153">Hodnota: `true`</span><span class="sxs-lookup"><span data-stu-id="3f254-153">Value: `true`</span></span>
3. <span data-ttu-id="3f254-154">**Uložit** hello nastavení a **restartujte** vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="3f254-154">**Save** hello settings and **Restart** your app.</span></span>

<span data-ttu-id="3f254-155">Hello Application Insights JavaScript SDK je nyní vsunout do každé webové stránky.</span><span class="sxs-lookup"><span data-stu-id="3f254-155">hello Application Insights JavaScript SDK is now injected into each web page.</span></span>

## <a name="monitor-a-live-iis-web-app"></a><span data-ttu-id="3f254-156">Monitorování živé webové aplikace IIS</span><span class="sxs-lookup"><span data-stu-id="3f254-156">Monitor a live IIS web app</span></span>

<span data-ttu-id="3f254-157">Pokud je vaše aplikace hostovaná na serveru služby IIS, povolte Application Insights pomocí Monitorování stavu.</span><span class="sxs-lookup"><span data-stu-id="3f254-157">If your app is hosted on an IIS server, enable Application Insights by using Status Monitor.</span></span>

1. <span data-ttu-id="3f254-158">Na webovém serveru služby IIS se přihlaste pomocí přihlašovacích údajů správce.</span><span class="sxs-lookup"><span data-stu-id="3f254-158">On your IIS web server, sign in with administrator credentials.</span></span>
2. <span data-ttu-id="3f254-159">Pokud ještě není nainstalovaný monitorování stavu Application Insights, stáhněte a spusťte hello [instalačního programu Sledování stavu](http://go.microsoft.com/fwlink/?LinkId=506648) (nebo spustit [instalačního programu webové platformy](https://www.microsoft.com/web/downloads/platform.aspx) a vyhledejte v něm přehled stavu aplikace. Monitorování).</span><span class="sxs-lookup"><span data-stu-id="3f254-159">If Application Insights Status Monitor is not already installed, download and run hello [Status Monitor installer](http://go.microsoft.com/fwlink/?LinkId=506648) (or run [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) and search in it for Application Insights Status Monitor).</span></span>
3. <span data-ttu-id="3f254-160">V monitorování stavu vyberte hello nainstalované webové aplikace nebo weby, které chcete toomonitor.</span><span class="sxs-lookup"><span data-stu-id="3f254-160">In Status Monitor, select hello installed web application or website that you want toomonitor.</span></span> <span data-ttu-id="3f254-161">Přihlaste se pomocí přihlašovacích údajů Azure.</span><span class="sxs-lookup"><span data-stu-id="3f254-161">Sign in with your Azure credentials.</span></span>

    <span data-ttu-id="3f254-162">Konfigurace prostředků hello místo, kam chcete výsledky hello toosee hello portálu Application Insights.</span><span class="sxs-lookup"><span data-stu-id="3f254-162">Configure hello resource where you want toosee hello results in hello Application Insights portal.</span></span> <span data-ttu-id="3f254-163">(Obvykle je nejlepší toocreate nový prostředek.</span><span class="sxs-lookup"><span data-stu-id="3f254-163">(Normally, it's best toocreate a new resource.</span></span> <span data-ttu-id="3f254-164">Vyberte existující prostředek, pokud už pro tuto aplikaci máte [webové testy][availability] dostupnosti nebo [monitorování klienta][client] .)</span><span class="sxs-lookup"><span data-stu-id="3f254-164">Select an existing resource if you already have [web tests][availability] or [client monitoring][client] for this app.)</span></span> 

    ![Vyberte aplikaci a prostředek.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-configAIC.png)

4. <span data-ttu-id="3f254-166">Restartujte službu IIS.</span><span class="sxs-lookup"><span data-stu-id="3f254-166">Restart IIS.</span></span>

    ![Zvolte restartování v horní části hello hello dialogového okna.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-restart.png)

    <span data-ttu-id="3f254-168">Webová služba bude na krátkou dobu přerušena.</span><span class="sxs-lookup"><span data-stu-id="3f254-168">Your web service is interrupted for a short while.</span></span>

## <a name="customize-monitoring-options"></a><span data-ttu-id="3f254-169">Přizpůsobení možností monitorování</span><span class="sxs-lookup"><span data-stu-id="3f254-169">Customize monitoring options</span></span>

<span data-ttu-id="3f254-170">Povolení Application Insights přidá knihovny DLL a soubor ApplicationInsights.config tooyour webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="3f254-170">Enabling Application Insights adds DLLs and ApplicationInsights.config tooyour web app.</span></span> <span data-ttu-id="3f254-171">Můžete [upravte soubor .config hello](app-insights-configuration-with-applicationinsights-config.md) toochange některé z možností hello.</span><span class="sxs-lookup"><span data-stu-id="3f254-171">You can [edit hello .config file](app-insights-configuration-with-applicationinsights-config.md) toochange some of hello options.</span></span>

## <a name="when-you-re-publish-your-app-re-enable-application-insights"></a><span data-ttu-id="3f254-172">Když opětovně publikujete aplikaci, znovu povolte Application Insights</span><span class="sxs-lookup"><span data-stu-id="3f254-172">When you re-publish your app, re-enable Application Insights</span></span>

<span data-ttu-id="3f254-173">Před znovu publikovat aplikace, vezměte v úvahu [přidání kódu toohello Application Insights v sadě Visual Studio][greenbrown].</span><span class="sxs-lookup"><span data-stu-id="3f254-173">Before you re-publish your app, consider [adding Application Insights toohello code in Visual Studio][greenbrown].</span></span> <span data-ttu-id="3f254-174">Získáte podrobnější telemetrie a hello možnost toowrite vlastní telemetrii.</span><span class="sxs-lookup"><span data-stu-id="3f254-174">You'll get more detailed telemetry and hello ability toowrite custom telemetry.</span></span>

<span data-ttu-id="3f254-175">Pokud chcete, aby toore-publikovat bez přidání Application Insights toohello kódu, uvědomte si, že proces nasazení hello může odstranit hello knihovny DLL a soubor ApplicationInsights.config z hello publikování webu.</span><span class="sxs-lookup"><span data-stu-id="3f254-175">If you want toore-publish without adding Application Insights toohello code, be aware that hello deployment process may delete hello DLLs and ApplicationInsights.config from hello published web site.</span></span> <span data-ttu-id="3f254-176">Proto:</span><span class="sxs-lookup"><span data-stu-id="3f254-176">Therefore:</span></span>

1. <span data-ttu-id="3f254-177">Pokud jste upravili soubor ApplicationInsights.config, pořiďte si jeho zálohu, než budete aplikaci znovu publikovat.</span><span class="sxs-lookup"><span data-stu-id="3f254-177">If you edited ApplicationInsights.config, take a copy of it before you re-publish your app.</span></span>
2. <span data-ttu-id="3f254-178">Znovu publikujte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3f254-178">Republish your app.</span></span>
3. <span data-ttu-id="3f254-179">Znovu povolte monitorování pomocí Application Insights.</span><span class="sxs-lookup"><span data-stu-id="3f254-179">Re-enable Application Insights monitoring.</span></span> <span data-ttu-id="3f254-180">(Použijte odpovídající metodu hello: hello Azure webové aplikace ovládacích panelů nebo hello monitorování stavu na hostiteli služby IIS.)</span><span class="sxs-lookup"><span data-stu-id="3f254-180">(Use hello appropriate method: either hello Azure web app control panel, or hello Status Monitor on an IIS host.)</span></span>
4. <span data-ttu-id="3f254-181">Obnovit všechny úpravy, které jste provedli v souboru .config hello.</span><span class="sxs-lookup"><span data-stu-id="3f254-181">Reinstate any edits you performed on hello .config file.</span></span>


## <a name="troubleshooting-runtime-configuration-of-application-insights"></a><span data-ttu-id="3f254-182">Řešení potíží s konfigurací modulu runtime služby Application Insights</span><span class="sxs-lookup"><span data-stu-id="3f254-182">Troubleshooting runtime configuration of Application Insights</span></span>

### <a name="cant-connect-no-telemetry"></a><span data-ttu-id="3f254-183">Nelze se připojit?</span><span class="sxs-lookup"><span data-stu-id="3f254-183">Can't connect?</span></span> <span data-ttu-id="3f254-184">Žádná telemetrie?</span><span class="sxs-lookup"><span data-stu-id="3f254-184">No telemetry?</span></span>

* <span data-ttu-id="3f254-185">Otevřete [hello nezbytné Odchozí porty](app-insights-ip-addresses.md#outgoing-ports) v toowork monitorování stavu tooallow brány firewall vašeho serveru.</span><span class="sxs-lookup"><span data-stu-id="3f254-185">Open [hello necessary outgoing ports](app-insights-ip-addresses.md#outgoing-ports) in your server's firewall tooallow Status Monitor toowork.</span></span>

* <span data-ttu-id="3f254-186">Otevřete monitorování stavu a vyberte svou aplikaci v levém podokně.</span><span class="sxs-lookup"><span data-stu-id="3f254-186">Open Status Monitor and select your application on left pane.</span></span> <span data-ttu-id="3f254-187">Zkontrolujte, zda existují jakékoli zprávy diagnostiky pro tuto aplikaci v části "Konfigurace oznámení" hello:</span><span class="sxs-lookup"><span data-stu-id="3f254-187">Check if there are any diagnostics messages for this application in hello "Configuration notifications" section:</span></span>

  ![Otevřete hello výkonu okno toosee žádost, dobu odezvy, závislosti a další data](./media/app-insights-monitor-performance-live-website-now/appinsights-status-monitor-diagnostics-message.png)
* <span data-ttu-id="3f254-189">Na serveru hello Pokud se zobrazí zpráva o "nedostatečných oprávněních", zkuste následující hello:</span><span class="sxs-lookup"><span data-stu-id="3f254-189">On hello server, if you see a message about "insufficient permissions", try hello following:</span></span>
  * <span data-ttu-id="3f254-190">Ve Správci služby IIS vyberte fond aplikací, otevřete **Upřesnit nastavení**a v části **Model procesu** Poznámka: hello identity.</span><span class="sxs-lookup"><span data-stu-id="3f254-190">In IIS Manager, select your application pool, open **Advanced Settings**, and under **Process Model** note hello identity.</span></span>
  * <span data-ttu-id="3f254-191">V Ovládacích panelech správy počítače přidejte tuto skupinu identity toohello Performance Monitor Users.</span><span class="sxs-lookup"><span data-stu-id="3f254-191">In Computer management control panel, add this identity toohello Performance Monitor Users group.</span></span>
* <span data-ttu-id="3f254-192">Pokud máte na serveru nainstalovaný MMA/SCOM (System Center Operations Manager), může u některých verzí dojít ke konfliktu.</span><span class="sxs-lookup"><span data-stu-id="3f254-192">If you have MMA/SCOM (Systems Center Operations Manager) installed on your server, some versions can conflict.</span></span> <span data-ttu-id="3f254-193">Odinstalujte SCOM a sledování stavu a znovu nainstalujte nejnovější verze hello.</span><span class="sxs-lookup"><span data-stu-id="3f254-193">Uninstall both SCOM and Status Monitor, and re-install hello latest versions.</span></span>
* <span data-ttu-id="3f254-194">Další informace najdete v tématu [Poradce při potížích][qna].</span><span class="sxs-lookup"><span data-stu-id="3f254-194">See [Troubleshooting][qna].</span></span>

## <a name="system-requirements"></a><span data-ttu-id="3f254-195">Systémové požadavky</span><span class="sxs-lookup"><span data-stu-id="3f254-195">System Requirements</span></span>
<span data-ttu-id="3f254-196">Podpora operačního systému pro sledování stavu Application Insights na serveru:</span><span class="sxs-lookup"><span data-stu-id="3f254-196">OS support for Application Insights Status Monitor on Server:</span></span>

* <span data-ttu-id="3f254-197">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="3f254-197">Windows Server 2008</span></span>
* <span data-ttu-id="3f254-198">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="3f254-198">Windows Server 2008 R2</span></span>
* <span data-ttu-id="3f254-199">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="3f254-199">Windows Server 2012</span></span>
* <span data-ttu-id="3f254-200">Windows server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="3f254-200">Windows server 2012 R2</span></span>
* <span data-ttu-id="3f254-201">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="3f254-201">Windows Server 2016</span></span>

<span data-ttu-id="3f254-202">s nejnovější aktualizací SP a rozhraním .NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="3f254-202">with latest SP and .NET Framework 4.5</span></span>

<span data-ttu-id="3f254-203">Na straně klienta hello: systém Windows 7, 8, 8.1 a 10, znovu pomocí rozhraní .NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="3f254-203">On hello client side: Windows 7, 8, 8.1 and 10, again with .NET Framework 4.5</span></span>

<span data-ttu-id="3f254-204">Podpora služby IIS je: IIS 7, 7.5, 8, 8.5 (je vyžadována služba IIS)</span><span class="sxs-lookup"><span data-stu-id="3f254-204">IIS support is: IIS 7, 7.5, 8, 8.5 (IIS is required)</span></span>

## <a name="automation-with-powershell"></a><span data-ttu-id="3f254-205">Automatizace v prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="3f254-205">Automation with PowerShell</span></span>
<span data-ttu-id="3f254-206">K zahájení a spuštění monitorování můžete na serveru služby IIS použít prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3f254-206">You can start and stop monitoring by using PowerShell on your IIS server.</span></span>

<span data-ttu-id="3f254-207">Nejdříve naimportujte modul Application Insights hello:</span><span class="sxs-lookup"><span data-stu-id="3f254-207">First import hello Application Insights module:</span></span>

`Import-Module 'C:\Program Files\Microsoft Application Insights\Status Monitor\PowerShell\Microsoft.Diagnostics.Agent.StatusMonitor.PowerShell.dll'`

<span data-ttu-id="3f254-208">Zjistěte, které aplikace se monitorují:</span><span class="sxs-lookup"><span data-stu-id="3f254-208">Find out which apps are being monitored:</span></span>

`Get-ApplicationInsightsMonitoringStatus [-Name appName]`

* <span data-ttu-id="3f254-209">`-Name`Hello (volitelné) název webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="3f254-209">`-Name` (Optional) hello name of a web app.</span></span>
* <span data-ttu-id="3f254-210">Zobrazí hello monitorování stavu Application Insights pro každou webovou aplikaci (nebo s názvem aplikace hello) v tomto serveru IIS.</span><span class="sxs-lookup"><span data-stu-id="3f254-210">Displays hello Application Insights monitoring status for each web app (or hello named app) in this IIS server.</span></span>
* <span data-ttu-id="3f254-211">Vrátí `ApplicationInsightsApplication` pro každou aplikaci:</span><span class="sxs-lookup"><span data-stu-id="3f254-211">Returns `ApplicationInsightsApplication` for each app:</span></span>

  * <span data-ttu-id="3f254-212">`SdkState==EnabledAfterDeployment`: Aplikace je monitorována a byla instrumentována v době běhu pomocí nástroje Monitor stavu hello nebo pomocí `Start-ApplicationInsightsMonitoring`.</span><span class="sxs-lookup"><span data-stu-id="3f254-212">`SdkState==EnabledAfterDeployment`: App is being monitored, and was instrumented at run time, either by hello Status Monitor tool, or by `Start-ApplicationInsightsMonitoring`.</span></span>
  * <span data-ttu-id="3f254-213">`SdkState==Disabled`: hello aplikace není instrumentována pro službu Application Insights.</span><span class="sxs-lookup"><span data-stu-id="3f254-213">`SdkState==Disabled`: hello app is not instrumented for Application Insights.</span></span> <span data-ttu-id="3f254-214">Buď nebyla nikdy instrumentována, nebo spuštění monitorování zakázal hello nástroje Monitor stavu nebo s `Stop-ApplicationInsightsMonitoring`.</span><span class="sxs-lookup"><span data-stu-id="3f254-214">Either it was never instrumented, or run-time monitoring was disabled with hello Status Monitor tool or with `Stop-ApplicationInsightsMonitoring`.</span></span>
  * <span data-ttu-id="3f254-215">`SdkState==EnabledByCodeInstrumentation`: hello aplikace byla instrumentována přidáním hello SDK toohello zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="3f254-215">`SdkState==EnabledByCodeInstrumentation`: hello app was instrumented by adding hello SDK toohello source code.</span></span> <span data-ttu-id="3f254-216">Její SDK nelze aktualizovat ani zastavit.</span><span class="sxs-lookup"><span data-stu-id="3f254-216">Its SDK cannot be updated or stopped.</span></span>
  * <span data-ttu-id="3f254-217">`SdkVersion`Zobrazuje verze hello používá pro monitorování této aplikace.</span><span class="sxs-lookup"><span data-stu-id="3f254-217">`SdkVersion` shows hello version in use for monitoring this app.</span></span>
  * <span data-ttu-id="3f254-218">`LatestAvailableSdkVersion`Zobrazuje hello verzi aktuálně k dispozici v galerii NuGet hello.</span><span class="sxs-lookup"><span data-stu-id="3f254-218">`LatestAvailableSdkVersion`shows hello version currently available on hello NuGet gallery.</span></span> <span data-ttu-id="3f254-219">verze toothis tooupgrade hello aplikace, použijte `Update-ApplicationInsightsMonitoring`.</span><span class="sxs-lookup"><span data-stu-id="3f254-219">tooupgrade hello app toothis version, use `Update-ApplicationInsightsMonitoring`.</span></span>

`Start-ApplicationInsightsMonitoring -Name appName -InstrumentationKey 00000000-000-000-000-0000000`

* <span data-ttu-id="3f254-220">`-Name`Název Hello hello aplikace ve službě IIS</span><span class="sxs-lookup"><span data-stu-id="3f254-220">`-Name` hello name of hello app in IIS</span></span>
* <span data-ttu-id="3f254-221">`-InstrumentationKey`Hello ikey prostředku Application Insights, kam chcete toobe hello výsledky zobrazí hello.</span><span class="sxs-lookup"><span data-stu-id="3f254-221">`-InstrumentationKey` hello ikey of hello Application Insights resource where you want hello results toobe displayed.</span></span>
* <span data-ttu-id="3f254-222">Tato rutina ovlivní pouze aplikace, které již nejsou instrumentovány – to znamená, SdkState==NotInstrumented.</span><span class="sxs-lookup"><span data-stu-id="3f254-222">This cmdlet only affects apps that are not already instrumented - that is, SdkState==NotInstrumented.</span></span>

    <span data-ttu-id="3f254-223">Hello rutina nemá vliv na aplikaci, která je již instrumentována.</span><span class="sxs-lookup"><span data-stu-id="3f254-223">hello cmdlet does not affect an app that is already instrumented.</span></span> <span data-ttu-id="3f254-224">Ji není důležité, zda text hello aplikace byla instrumentována v okamžiku sestavení přidáním hello SDK toohello kód, nebo na dobu běhu předchozí pomocí této rutiny.</span><span class="sxs-lookup"><span data-stu-id="3f254-224">It does not matter whether hello app was instrumented at build time by adding hello SDK toohello code, or at run time by a previous use of this cmdlet.</span></span>

    <span data-ttu-id="3f254-225">Hello SDK verze použitá tooinstrument hello aplikace je, že hello verzi, která byla nedávno většina toothis server stáhli.</span><span class="sxs-lookup"><span data-stu-id="3f254-225">hello SDK version used tooinstrument hello app is hello version that was most recently downloaded toothis server.</span></span>

    <span data-ttu-id="3f254-226">toodownload hello nejnovější verzi, použijte příkaz Update-ApplicationInsightsVersion.</span><span class="sxs-lookup"><span data-stu-id="3f254-226">toodownload hello latest version, use Update-ApplicationInsightsVersion.</span></span>
* <span data-ttu-id="3f254-227">V případě úspěchu vrátí `ApplicationInsightsApplication`.</span><span class="sxs-lookup"><span data-stu-id="3f254-227">Returns `ApplicationInsightsApplication` on success.</span></span> <span data-ttu-id="3f254-228">Pokud se nezdaří, zaprotokoluje trasování toostderr.</span><span class="sxs-lookup"><span data-stu-id="3f254-228">If it fails, it logs a trace toostderr.</span></span>

          Name                      : Default Web Site/WebApp1
          InstrumentationKey        : 00000000-0000-0000-0000-000000000000
          ProfilerState             : ApplicationInsights
          SdkState                  : EnabledAfterDeployment
          SdkVersion                : 1.2.1
          LatestAvailableSdkVersion : 1.2.3

`Stop-ApplicationInsightsMonitoring [-Name appName | -All]`

* <span data-ttu-id="3f254-229">`-Name`Název Hello aplikace ve službě IIS</span><span class="sxs-lookup"><span data-stu-id="3f254-229">`-Name` hello name of an app in IIS</span></span>
* <span data-ttu-id="3f254-230">`-All` Zastaví monitorování všech aplikací v tomto serveru IIS, pro který platí, že `SdkState==EnabledAfterDeployment`.</span><span class="sxs-lookup"><span data-stu-id="3f254-230">`-All` Stops monitoring all apps in this IIS server for which `SdkState==EnabledAfterDeployment`</span></span>
* <span data-ttu-id="3f254-231">Zastaví monitorování hello dané aplikace a odebere instrumentace.</span><span class="sxs-lookup"><span data-stu-id="3f254-231">Stops monitoring hello specified apps and removes instrumentation.</span></span> <span data-ttu-id="3f254-232">Funguje pouze pro aplikace, které byly instrumentovány v běhu pomocí hello nástroje pro monitorování stavu nebo příkazu Start-ApplicationInsightsApplication.</span><span class="sxs-lookup"><span data-stu-id="3f254-232">It only works for apps that have been instrumented at run-time using hello Status Monitoring tool or Start-ApplicationInsightsApplication.</span></span> <span data-ttu-id="3f254-233">(`SdkState==EnabledAfterDeployment`)</span><span class="sxs-lookup"><span data-stu-id="3f254-233">(`SdkState==EnabledAfterDeployment`)</span></span>
* <span data-ttu-id="3f254-234">Vrátí ApplicationInsightsApplication.</span><span class="sxs-lookup"><span data-stu-id="3f254-234">Returns ApplicationInsightsApplication.</span></span>

<span data-ttu-id="3f254-235">`Update-ApplicationInsightsMonitoring -Name appName [-InstrumentationKey "0000000-0000-000-000-0000"`]</span><span class="sxs-lookup"><span data-stu-id="3f254-235">`Update-ApplicationInsightsMonitoring -Name appName [-InstrumentationKey "0000000-0000-000-000-0000"`]</span></span>

* <span data-ttu-id="3f254-236">`-Name`: hello název webové aplikace ve službě IIS.</span><span class="sxs-lookup"><span data-stu-id="3f254-236">`-Name`: hello name of a web app in IIS.</span></span>
* <span data-ttu-id="3f254-237">`-InstrumentationKey` (Volitelné) Pomocí této toochange hello prostředků toowhich hello telemetrii aplikace je odeslán.</span><span class="sxs-lookup"><span data-stu-id="3f254-237">`-InstrumentationKey` (Optional.) Use this toochange hello resource toowhich hello app's telemetry is sent.</span></span>
* <span data-ttu-id="3f254-238">Tato rutina:</span><span class="sxs-lookup"><span data-stu-id="3f254-238">This cmdlet:</span></span>
  * <span data-ttu-id="3f254-239">Upgrady hello s názvem aplikace toohello verzi hello SDK naposledy stažené toothis počítače.</span><span class="sxs-lookup"><span data-stu-id="3f254-239">Upgrades hello named app toohello version of hello SDK most recently downloaded toothis machine.</span></span> <span data-ttu-id="3f254-240">(Funguje pouze v případě `SdkState==EnabledAfterDeployment`)</span><span class="sxs-lookup"><span data-stu-id="3f254-240">(Only works if `SdkState==EnabledAfterDeployment`)</span></span>
  * <span data-ttu-id="3f254-241">Pokud jste zadali kód instrumentace, změněnou konfigurací toosend telemetrie toohello prostředek s tímto klíčem, je hello s názvem aplikace.</span><span class="sxs-lookup"><span data-stu-id="3f254-241">If you provide an instrumentation key, hello named app is reconfigured toosend telemetry toohello resource with that key.</span></span> <span data-ttu-id="3f254-242">(Funguje v případě `SdkState != Disabled`)</span><span class="sxs-lookup"><span data-stu-id="3f254-242">(Works if `SdkState != Disabled`)</span></span>

`Update-ApplicationInsightsVersion`

* <span data-ttu-id="3f254-243">Stáhne hello nejnovější Application Insights SDK toohello serveru.</span><span class="sxs-lookup"><span data-stu-id="3f254-243">Downloads hello latest Application Insights SDK toohello server.</span></span>

## <span data-ttu-id="3f254-244"><a name="questions"></a>Dotazy týkající se Monitorování stavu</span><span class="sxs-lookup"><span data-stu-id="3f254-244"><a name="questions"></a>Questions about Status Monitor</span></span>

### <a name="what-is-status-monitor"></a><span data-ttu-id="3f254-245">Co je Monitorování stavu?</span><span class="sxs-lookup"><span data-stu-id="3f254-245">What is Status Monitor?</span></span>

<span data-ttu-id="3f254-246">Desktopová aplikace, kterou instalujete s webovým serverem IIS.</span><span class="sxs-lookup"><span data-stu-id="3f254-246">A desktop application that you install in your IIS web server.</span></span> <span data-ttu-id="3f254-247">Pomáhá provádět instrumentaci a konfiguraci webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="3f254-247">It helps you instrument and configure web apps.</span></span> 

### <a name="when-do-i-use-status-monitor"></a><span data-ttu-id="3f254-248">Kdy použít Monitorování stavu?</span><span class="sxs-lookup"><span data-stu-id="3f254-248">When do I use Status Monitor?</span></span>

* <span data-ttu-id="3f254-249">tooinstrument žádné webové aplikace, která běží na serveru se službou IIS - i v případě, že je již spuštěna.</span><span class="sxs-lookup"><span data-stu-id="3f254-249">tooinstrument any web app that is running on your IIS server - even if it is already running.</span></span>
* <span data-ttu-id="3f254-250">Další telemetrické tooenable pro webové aplikace, které byly [vytvořené s nástroji hello Application Insights SDK](app-insights-asp-net.md) v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="3f254-250">tooenable additional telemetry for web apps that have been [built with hello Application Insights SDK](app-insights-asp-net.md) at compile time.</span></span> 

### <a name="can-i-close-it-after-it-runs"></a><span data-ttu-id="3f254-251">Můžu ji po spuštění zavřít?</span><span class="sxs-lookup"><span data-stu-id="3f254-251">Can I close it after it runs?</span></span>

<span data-ttu-id="3f254-252">Ano.</span><span class="sxs-lookup"><span data-stu-id="3f254-252">Yes.</span></span> <span data-ttu-id="3f254-253">Poté, co se má instrumentována hello webů, které vyberete, můžete ho zavřít.</span><span class="sxs-lookup"><span data-stu-id="3f254-253">After it has instrumented hello websites you select, you can close it.</span></span>

<span data-ttu-id="3f254-254">Sama o sobě telemetrii neshromažďuje.</span><span class="sxs-lookup"><span data-stu-id="3f254-254">It doesn't collect telemetry by itself.</span></span> <span data-ttu-id="3f254-255">Právě nakonfiguruje hello webové aplikace a nastaví některá oprávnění.</span><span class="sxs-lookup"><span data-stu-id="3f254-255">It just configures hello web apps and sets some permissions.</span></span>

### <a name="what-does-status-monitor-do"></a><span data-ttu-id="3f254-256">K čemu Monitorování stavu slouží?</span><span class="sxs-lookup"><span data-stu-id="3f254-256">What does Status Monitor do?</span></span>

<span data-ttu-id="3f254-257">Když vyberete webovou aplikaci pro monitorování stavu tooinstrument:</span><span class="sxs-lookup"><span data-stu-id="3f254-257">When you select a web app for Status Monitor tooinstrument:</span></span>

* <span data-ttu-id="3f254-258">Stáhne a umístí hello Application Insights sestavení a souboru .config složku binárních souborů hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="3f254-258">Downloads and places hello Application Insights assemblies and .config file in hello web app's binaries folder.</span></span>
* <span data-ttu-id="3f254-259">Upraví `web.config` tooadd hello Application Insights na požadavek HTTP sledování modulu.</span><span class="sxs-lookup"><span data-stu-id="3f254-259">Modifies `web.config` tooadd hello Application Insights HTTP tracking module.</span></span>
* <span data-ttu-id="3f254-260">Umožňuje CLR profilace toocollect závislostí volání.</span><span class="sxs-lookup"><span data-stu-id="3f254-260">Enables CLR profiling toocollect dependency calls.</span></span>

### <a name="do-i-need-toorun-status-monitor-whenever-i-update-hello-app"></a><span data-ttu-id="3f254-261">Potřebuji toorun monitorování stavu, vždy, když aktualizuji hello aplikace?</span><span class="sxs-lookup"><span data-stu-id="3f254-261">Do I need toorun Status Monitor whenever I update hello app?</span></span>

<span data-ttu-id="3f254-262">Ne, pokud provádíte opakované nasazení postupně.</span><span class="sxs-lookup"><span data-stu-id="3f254-262">Not if you redeploy incrementally.</span></span> 

<span data-ttu-id="3f254-263">Pokud vyberete možnost, odstraňte stávající soubory"hello v hello publikování procesu, potřebovali byste toore spustit monitorování stavu tooconfigure Application Insights.</span><span class="sxs-lookup"><span data-stu-id="3f254-263">If you select hello 'delete existing files' option in hello publish process, you would need toore-run Status Monitor tooconfigure Application Insights.</span></span>

### <a name="what-telemetry-is-collected"></a><span data-ttu-id="3f254-264">Jaké telemetrická data se shromažďují?</span><span class="sxs-lookup"><span data-stu-id="3f254-264">What telemetry is collected?</span></span>

<span data-ttu-id="3f254-265">Pro aplikace instrumentované pouze za běhu pomocí Monitorování stavu:</span><span class="sxs-lookup"><span data-stu-id="3f254-265">For applications that you instrument only at run-time by using Status Monitor:</span></span>

* <span data-ttu-id="3f254-266">Požadavky HTTP</span><span class="sxs-lookup"><span data-stu-id="3f254-266">HTTP requests</span></span>
* <span data-ttu-id="3f254-267">Volání toodependencies</span><span class="sxs-lookup"><span data-stu-id="3f254-267">Calls toodependencies</span></span>
* <span data-ttu-id="3f254-268">Výjimky</span><span class="sxs-lookup"><span data-stu-id="3f254-268">Exceptions</span></span>
* <span data-ttu-id="3f254-269">Čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="3f254-269">Performance counters</span></span>

<span data-ttu-id="3f254-270">Pro aplikace již instrumentované v době kompilace:</span><span class="sxs-lookup"><span data-stu-id="3f254-270">For applications already instrumented at compile time:</span></span>

 * <span data-ttu-id="3f254-271">Čítače procesů</span><span class="sxs-lookup"><span data-stu-id="3f254-271">Process counters.</span></span>
 * <span data-ttu-id="3f254-272">Volání závislostí (.NET 4.5); návratové hodnoty ve voláních závislostí (.NET 4.6)</span><span class="sxs-lookup"><span data-stu-id="3f254-272">Dependency calls (.NET 4.5); return values in dependency calls (.NET 4.6).</span></span>
 * <span data-ttu-id="3f254-273">Hodnoty trasování zásobníku výjimek</span><span class="sxs-lookup"><span data-stu-id="3f254-273">Exception stack trace values.</span></span>

[<span data-ttu-id="3f254-274">Další informace</span><span class="sxs-lookup"><span data-stu-id="3f254-274">Learn more</span></span>](http://apmtips.com/blog/2016/11/18/how-application-insights-status-monitor-not-monitors-dependencies/)

## <a name="video"></a><span data-ttu-id="3f254-275">Video</span><span class="sxs-lookup"><span data-stu-id="3f254-275">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <span data-ttu-id="3f254-276"><a name="next"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="3f254-276"><a name="next"></a>Next steps</span></span>

<span data-ttu-id="3f254-277">Zobrazení telemetrických dat:</span><span class="sxs-lookup"><span data-stu-id="3f254-277">View your telemetry:</span></span>

* <span data-ttu-id="3f254-278">[Zkoumat metriky](app-insights-metrics-explorer.md) toomonitor výkonu a využití</span><span class="sxs-lookup"><span data-stu-id="3f254-278">[Explore metrics](app-insights-metrics-explorer.md) toomonitor performance and usage</span></span>
* <span data-ttu-id="3f254-279">[Hledání událostí a protokolů] [ diagnostic] toodiagnose problémy</span><span class="sxs-lookup"><span data-stu-id="3f254-279">[Search events and logs][diagnostic] toodiagnose problems</span></span>
* <span data-ttu-id="3f254-280">[Analýzy](app-insights-analytics.md) pro pokročilejší dotazy</span><span class="sxs-lookup"><span data-stu-id="3f254-280">[Analytics](app-insights-analytics.md) for more advanced queries</span></span>
* [<span data-ttu-id="3f254-281">Vytváření řídicích panelů</span><span class="sxs-lookup"><span data-stu-id="3f254-281">Create dashboards</span></span>](app-insights-dashboards.md)

<span data-ttu-id="3f254-282">Přidání další telemetrie:</span><span class="sxs-lookup"><span data-stu-id="3f254-282">Add more telemetry:</span></span>

* <span data-ttu-id="3f254-283">[Vytvářejte webové testy] [ availability] toomake, zda web zůstává živý.</span><span class="sxs-lookup"><span data-stu-id="3f254-283">[Create web tests][availability] toomake sure your site stays live.</span></span>
* <span data-ttu-id="3f254-284">[Přidat telemetrie webového klienta] [ usage] toosee výjimek z kódu webové stránky a toolet vložení trasovacího volání.</span><span class="sxs-lookup"><span data-stu-id="3f254-284">[Add web client telemetry][usage] toosee exceptions from web page code and toolet you insert trace calls.</span></span>
* <span data-ttu-id="3f254-285">[Přidejte Application Insights SDK tooyour kód] [ greenbrown] , aby mohli vložit trasování a protokolování volání</span><span class="sxs-lookup"><span data-stu-id="3f254-285">[Add Application Insights SDK tooyour code][greenbrown] so that you can insert trace and log calls</span></span>

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[availability]: app-insights-monitor-web-app-availability.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-asp-net.md
[qna]: app-insights-troubleshoot-faq.md
[roles]: app-insights-resources-roles-access-control.md
[usage]: app-insights-javascript.md
