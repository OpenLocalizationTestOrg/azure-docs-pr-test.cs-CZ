---
title: "Monitorování živé webové aplikace v ASP.NET pomocí Azure Application Insights | Dokumentace Microsoftu"
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
ms.openlocfilehash: d07a0c81f89100c378456bbea8dca1c009cc8d77
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="instrument-web-apps-at-runtime-with-application-insights"></a><span data-ttu-id="9e790-104">Instrumentace webových aplikací za běhu pomocí nástrojů Application Insights</span><span class="sxs-lookup"><span data-stu-id="9e790-104">Instrument web apps at runtime with Application Insights</span></span>


<span data-ttu-id="9e790-105">Azure Application Insights vám umožňuje instrumentovat živou webovou aplikaci, aniž byste museli upravovat nebo znovu nasazovat kód.</span><span class="sxs-lookup"><span data-stu-id="9e790-105">You can instrument a live web app with Azure Application Insights, without having to modify or redeploy your code.</span></span> <span data-ttu-id="9e790-106">Pokud vaše aplikace hostuje místní server služby IIS, nainstalujte Monitorování stavu.</span><span class="sxs-lookup"><span data-stu-id="9e790-106">If your apps are hosted by an on-premises IIS server, install Status Monitor.</span></span> <span data-ttu-id="9e790-107">Pokud se jedná o webové aplikace Azure nebo pokud běží ve virtuálním počítači Azure, můžete monitorování pomocí Application Insights zapnout z ovládacího panelu Azure.</span><span class="sxs-lookup"><span data-stu-id="9e790-107">If they're Azure web apps or run in an Azure VM, you can switch on Application Insights monitoring from the Azure control panel.</span></span> <span data-ttu-id="9e790-108">(Existují i samostatné články o instrumentaci [živých webových aplikací J2EE](app-insights-java-live.md) a [Azure Cloud Services](app-insights-cloudservices.md).) Budete potřebovat předplatné [Microsoft Azure](http://azure.com).</span><span class="sxs-lookup"><span data-stu-id="9e790-108">(There are also separate articles about instrumenting [live J2EE web apps](app-insights-java-live.md) and [Azure Cloud Services](app-insights-cloudservices.md).) You need a [Microsoft Azure](http://azure.com) subscription.</span></span>

![ukázkové grafy](./media/app-insights-monitor-performance-live-website-now/10-intro.png)

<span data-ttu-id="9e790-110">Můžete si vybrat ze tří způsobů, jak u webových aplikací .NET použít službu Application Insights:</span><span class="sxs-lookup"><span data-stu-id="9e790-110">You have a choice of three routes to apply Application Insights to your .NET web applications:</span></span>

* <span data-ttu-id="9e790-111">**Čas sestavení:** [Přidejte Application Insights SDK][greenbrown] do kódu webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="9e790-111">**Build time:** [Add the Application Insights SDK][greenbrown] to your web app code.</span></span>
* <span data-ttu-id="9e790-112">**Za běhu:** Podle níže popsaného postupu proveďte instrumentaci webové aplikace na serveru, aniž byste museli znovu sestavovat a nasazovat kód.</span><span class="sxs-lookup"><span data-stu-id="9e790-112">**Run time:** Instrument your web app on the server, as described below, without rebuilding and redeploying the code.</span></span>
* <span data-ttu-id="9e790-113">**Obojí:** Přidejte do kódu webové aplikace sadu SDK a zároveň uplatněte rozšíření za běhu.</span><span class="sxs-lookup"><span data-stu-id="9e790-113">**Both:** Build the SDK into your web app code, and also apply the run-time extensions.</span></span> <span data-ttu-id="9e790-114">Získáte to nejlepší z obou možností.</span><span class="sxs-lookup"><span data-stu-id="9e790-114">Get the best of both options.</span></span>

<span data-ttu-id="9e790-115">Tady je rekapitulace toho, co každý způsob přináší:</span><span class="sxs-lookup"><span data-stu-id="9e790-115">Here's a summary of what you get by each route:</span></span>

|  | <span data-ttu-id="9e790-116">Při sestavení</span><span class="sxs-lookup"><span data-stu-id="9e790-116">Build time</span></span> | <span data-ttu-id="9e790-117">Za běhu</span><span class="sxs-lookup"><span data-stu-id="9e790-117">Run time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9e790-118">Požadavky a výjimky</span><span class="sxs-lookup"><span data-stu-id="9e790-118">Requests & exceptions</span></span> |<span data-ttu-id="9e790-119">Ano</span><span class="sxs-lookup"><span data-stu-id="9e790-119">Yes</span></span> |<span data-ttu-id="9e790-120">Ano</span><span class="sxs-lookup"><span data-stu-id="9e790-120">Yes</span></span> |
| [<span data-ttu-id="9e790-121">Podrobnější výjimky</span><span class="sxs-lookup"><span data-stu-id="9e790-121">More detailed exceptions</span></span>](app-insights-asp-net-exceptions.md) | |<span data-ttu-id="9e790-122">Ano</span><span class="sxs-lookup"><span data-stu-id="9e790-122">Yes</span></span> |
| [<span data-ttu-id="9e790-123">Diagnostika závislostí</span><span class="sxs-lookup"><span data-stu-id="9e790-123">Dependency diagnostics</span></span>](app-insights-asp-net-dependencies.md) |<span data-ttu-id="9e790-124">Na platformě .NET 4.6+, ale méně podrobná</span><span class="sxs-lookup"><span data-stu-id="9e790-124">On .NET 4.6+, but less detail</span></span> |<span data-ttu-id="9e790-125">Ano, úplné podrobnosti: kódy výsledků, text příkazu SQL, příkaz HTTP</span><span class="sxs-lookup"><span data-stu-id="9e790-125">Yes, full detail: result codes, SQL command text, HTTP verb</span></span>|
| [<span data-ttu-id="9e790-126">Čítače výkonu systému</span><span class="sxs-lookup"><span data-stu-id="9e790-126">System performance counters</span></span>](app-insights-performance-counters.md) |<span data-ttu-id="9e790-127">Ano</span><span class="sxs-lookup"><span data-stu-id="9e790-127">Yes</span></span> |<span data-ttu-id="9e790-128">Ano</span><span class="sxs-lookup"><span data-stu-id="9e790-128">Yes</span></span> |
| <span data-ttu-id="9e790-129">[Rozhraní API pro vlastní telemetrii][api]</span><span class="sxs-lookup"><span data-stu-id="9e790-129">[API for custom telemetry][api]</span></span> |<span data-ttu-id="9e790-130">Ano</span><span class="sxs-lookup"><span data-stu-id="9e790-130">Yes</span></span> |<span data-ttu-id="9e790-131">Ne</span><span class="sxs-lookup"><span data-stu-id="9e790-131">No</span></span> |
| [<span data-ttu-id="9e790-132">Integrace protokolu trasování</span><span class="sxs-lookup"><span data-stu-id="9e790-132">Trace log integration</span></span>](app-insights-asp-net-trace-logs.md) |<span data-ttu-id="9e790-133">Ano</span><span class="sxs-lookup"><span data-stu-id="9e790-133">Yes</span></span> |<span data-ttu-id="9e790-134">Ne</span><span class="sxs-lookup"><span data-stu-id="9e790-134">No</span></span> |
| [<span data-ttu-id="9e790-135">Zobrazení stránky a uživatelská data</span><span class="sxs-lookup"><span data-stu-id="9e790-135">Page view & user data</span></span>](app-insights-javascript.md) |<span data-ttu-id="9e790-136">Ano</span><span class="sxs-lookup"><span data-stu-id="9e790-136">Yes</span></span> |<span data-ttu-id="9e790-137">Ne</span><span class="sxs-lookup"><span data-stu-id="9e790-137">No</span></span> |
| <span data-ttu-id="9e790-138">Nutnost znovu sestavit kód</span><span class="sxs-lookup"><span data-stu-id="9e790-138">Need to rebuild code</span></span> |<span data-ttu-id="9e790-139">Ano</span><span class="sxs-lookup"><span data-stu-id="9e790-139">Yes</span></span> | <span data-ttu-id="9e790-140">Ne</span><span class="sxs-lookup"><span data-stu-id="9e790-140">No</span></span> |


## <a name="monitor-a-live-azure-web-app"></a><span data-ttu-id="9e790-141">Monitorování živé webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="9e790-141">Monitor a live Azure web app</span></span>

<span data-ttu-id="9e790-142">Pokud vaše aplikace běží jako webová služba Azure, monitorování zapnete následovně:</span><span class="sxs-lookup"><span data-stu-id="9e790-142">If your application is running as an Azure web service, here's how to switch on monitoring:</span></span>

* <span data-ttu-id="9e790-143">Na ovládacím panelu aplikace v Azure vyberte Application Insights.</span><span class="sxs-lookup"><span data-stu-id="9e790-143">Select Application Insights on the app's control panel in Azure.</span></span>

    ![Nastavení Application Insights pro webovou aplikaci Azure](./media/app-insights-monitor-performance-live-website-now/azure-web-setup.png)
* <span data-ttu-id="9e790-145">Až se otevře stránka se souhrnem Application Insights, kliknutím na odkaz v dolní části otevřete úplný prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="9e790-145">When the Application Insights summary page opens, click the link at the bottom to open the full Application Insights resource.</span></span>

    ![Proklikání se k Application Insights](./media/app-insights-monitor-performance-live-website-now/azure-web-view-more.png)

<span data-ttu-id="9e790-147">[Monitorování cloudových aplikací a aplikací virtuálních počítačů](app-insights-azure.md).</span><span class="sxs-lookup"><span data-stu-id="9e790-147">[Monitoring Cloud and VM apps](app-insights-azure.md).</span></span>

### <a name="enable-client-side-monitoring-in-azure"></a><span data-ttu-id="9e790-148">Povolení monitorování na straně klienta v Azure</span><span class="sxs-lookup"><span data-stu-id="9e790-148">Enable client-side monitoring in Azure</span></span>

<span data-ttu-id="9e790-149">Pokud jste v Azure povolili nástroje Application Insights, můžete přidat zobrazení stránky a telemetrii uživatelů.</span><span class="sxs-lookup"><span data-stu-id="9e790-149">If you have enabled Application Insights in Azure, you can add page view and user telemetry.</span></span>

1. <span data-ttu-id="9e790-150">Klikněte na Nastavení > Nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="9e790-150">Select Settings > Application Settings</span></span>
2.  <span data-ttu-id="9e790-151">V části Nastavení aplikace přidejte novou dvojici klíče a hodnoty:</span><span class="sxs-lookup"><span data-stu-id="9e790-151">Under App Settings, add a new key value pair:</span></span> 
   
    <span data-ttu-id="9e790-152">Klíč: `APPINSIGHTS_JAVASCRIPT_ENABLED`</span><span class="sxs-lookup"><span data-stu-id="9e790-152">Key: `APPINSIGHTS_JAVASCRIPT_ENABLED`</span></span> 
    
    <span data-ttu-id="9e790-153">Hodnota: `true`</span><span class="sxs-lookup"><span data-stu-id="9e790-153">Value: `true`</span></span>
3. <span data-ttu-id="9e790-154">Kliknutím na **Uložit** uložte nastavení a kliknutím na **Restartovat** restartujte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9e790-154">**Save** the settings and **Restart** your app.</span></span>

<span data-ttu-id="9e790-155">Každá webová stránka má teď vloženou sadu SDK Application Insights JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9e790-155">The Application Insights JavaScript SDK is now injected into each web page.</span></span>

## <a name="monitor-a-live-iis-web-app"></a><span data-ttu-id="9e790-156">Monitorování živé webové aplikace IIS</span><span class="sxs-lookup"><span data-stu-id="9e790-156">Monitor a live IIS web app</span></span>

<span data-ttu-id="9e790-157">Pokud je vaše aplikace hostovaná na serveru služby IIS, povolte Application Insights pomocí Monitorování stavu.</span><span class="sxs-lookup"><span data-stu-id="9e790-157">If your app is hosted on an IIS server, enable Application Insights by using Status Monitor.</span></span>

1. <span data-ttu-id="9e790-158">Na webovém serveru služby IIS se přihlaste pomocí přihlašovacích údajů správce.</span><span class="sxs-lookup"><span data-stu-id="9e790-158">On your IIS web server, sign in with administrator credentials.</span></span>
2. <span data-ttu-id="9e790-159">Pokud Monitorování stavu Application Insights ještě není nainstalované, stáhněte si a spusťte [instalační program Monitorování stavu](http://go.microsoft.com/fwlink/?LinkId=506648) (nebo spusťte [Instalaci webové platformy](https://www.microsoft.com/web/downloads/platform.aspx) a vyhledejte v ní Monitorování stavu Application Insights).</span><span class="sxs-lookup"><span data-stu-id="9e790-159">If Application Insights Status Monitor is not already installed, download and run the [Status Monitor installer](http://go.microsoft.com/fwlink/?LinkId=506648) (or run [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) and search in it for Application Insights Status Monitor).</span></span>
3. <span data-ttu-id="9e790-160">V Monitorování stavu vyberte nainstalovanou webovou aplikaci nebo web, které chcete monitorovat.</span><span class="sxs-lookup"><span data-stu-id="9e790-160">In Status Monitor, select the installed web application or website that you want to monitor.</span></span> <span data-ttu-id="9e790-161">Přihlaste se pomocí přihlašovacích údajů Azure.</span><span class="sxs-lookup"><span data-stu-id="9e790-161">Sign in with your Azure credentials.</span></span>

    <span data-ttu-id="9e790-162">Nakonfigurujte prostředek,ve kterém chcete zobrazovat výsledky na portálu Application Insights.</span><span class="sxs-lookup"><span data-stu-id="9e790-162">Configure the resource where you want to see the results in the Application Insights portal.</span></span> <span data-ttu-id="9e790-163">(Obvykle je nejlepší vytvořit nový prostředek.</span><span class="sxs-lookup"><span data-stu-id="9e790-163">(Normally, it's best to create a new resource.</span></span> <span data-ttu-id="9e790-164">Vyberte existující prostředek, pokud už pro tuto aplikaci máte [webové testy][availability] dostupnosti nebo [monitorování klienta][client] .)</span><span class="sxs-lookup"><span data-stu-id="9e790-164">Select an existing resource if you already have [web tests][availability] or [client monitoring][client] for this app.)</span></span> 

    ![Vyberte aplikaci a prostředek.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-configAIC.png)

4. <span data-ttu-id="9e790-166">Restartujte službu IIS.</span><span class="sxs-lookup"><span data-stu-id="9e790-166">Restart IIS.</span></span>

    ![Zvolte restartování v horní části dialogového okna.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-restart.png)

    <span data-ttu-id="9e790-168">Webová služba bude na krátkou dobu přerušena.</span><span class="sxs-lookup"><span data-stu-id="9e790-168">Your web service is interrupted for a short while.</span></span>

## <a name="customize-monitoring-options"></a><span data-ttu-id="9e790-169">Přizpůsobení možností monitorování</span><span class="sxs-lookup"><span data-stu-id="9e790-169">Customize monitoring options</span></span>

<span data-ttu-id="9e790-170">Povolením Application Insights se do webové aplikace přidají knihovny DLL a soubor ApplicationInsights.config.</span><span class="sxs-lookup"><span data-stu-id="9e790-170">Enabling Application Insights adds DLLs and ApplicationInsights.config to your web app.</span></span> <span data-ttu-id="9e790-171">Můžete [upravit soubor .config](app-insights-configuration-with-applicationinsights-config.md) a změnit některé možnosti.</span><span class="sxs-lookup"><span data-stu-id="9e790-171">You can [edit the .config file](app-insights-configuration-with-applicationinsights-config.md) to change some of the options.</span></span>

## <a name="when-you-re-publish-your-app-re-enable-application-insights"></a><span data-ttu-id="9e790-172">Když opětovně publikujete aplikaci, znovu povolte Application Insights</span><span class="sxs-lookup"><span data-stu-id="9e790-172">When you re-publish your app, re-enable Application Insights</span></span>

<span data-ttu-id="9e790-173">Před opětovným publikováním aplikace zvažte [přidání Application Insights do kódu v sadě Visual Studio][greenbrown].</span><span class="sxs-lookup"><span data-stu-id="9e790-173">Before you re-publish your app, consider [adding Application Insights to the code in Visual Studio][greenbrown].</span></span> <span data-ttu-id="9e790-174">Získáte tak podrobnější telemetrii a možnost zapisovat vlastní telemetrická data.</span><span class="sxs-lookup"><span data-stu-id="9e790-174">You'll get more detailed telemetry and the ability to write custom telemetry.</span></span>

<span data-ttu-id="9e790-175">Pokud chcete znovu publikovat aniž byste přidali Application Insights do kódu, mějte na paměti, že proces nasazení může odstranit knihovny DLL a soubor ApplicationInsights.config z publikovaného webu.</span><span class="sxs-lookup"><span data-stu-id="9e790-175">If you want to re-publish without adding Application Insights to the code, be aware that the deployment process may delete the DLLs and ApplicationInsights.config from the published web site.</span></span> <span data-ttu-id="9e790-176">Proto:</span><span class="sxs-lookup"><span data-stu-id="9e790-176">Therefore:</span></span>

1. <span data-ttu-id="9e790-177">Pokud jste upravili soubor ApplicationInsights.config, pořiďte si jeho zálohu, než budete aplikaci znovu publikovat.</span><span class="sxs-lookup"><span data-stu-id="9e790-177">If you edited ApplicationInsights.config, take a copy of it before you re-publish your app.</span></span>
2. <span data-ttu-id="9e790-178">Znovu publikujte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9e790-178">Republish your app.</span></span>
3. <span data-ttu-id="9e790-179">Znovu povolte monitorování pomocí Application Insights.</span><span class="sxs-lookup"><span data-stu-id="9e790-179">Re-enable Application Insights monitoring.</span></span> <span data-ttu-id="9e790-180">(Použijte vhodnou metodu: ovládací panel webové aplikace Azure, Monitorování stavu nebo hostitele služby IIS.)</span><span class="sxs-lookup"><span data-stu-id="9e790-180">(Use the appropriate method: either the Azure web app control panel, or the Status Monitor on an IIS host.)</span></span>
4. <span data-ttu-id="9e790-181">Obnovte veškeré úpravy, které jste provedli v souboru .config.</span><span class="sxs-lookup"><span data-stu-id="9e790-181">Reinstate any edits you performed on the .config file.</span></span>


## <a name="troubleshooting-runtime-configuration-of-application-insights"></a><span data-ttu-id="9e790-182">Řešení potíží s konfigurací modulu runtime služby Application Insights</span><span class="sxs-lookup"><span data-stu-id="9e790-182">Troubleshooting runtime configuration of Application Insights</span></span>

### <a name="cant-connect-no-telemetry"></a><span data-ttu-id="9e790-183">Nelze se připojit?</span><span class="sxs-lookup"><span data-stu-id="9e790-183">Can't connect?</span></span> <span data-ttu-id="9e790-184">Žádná telemetrie?</span><span class="sxs-lookup"><span data-stu-id="9e790-184">No telemetry?</span></span>

* <span data-ttu-id="9e790-185">Otevřete v bráně firewall vašeho serveru [potřebné odchozí porty](app-insights-ip-addresses.md#outgoing-ports), aby Monitorování stavu mohlo fungovat.</span><span class="sxs-lookup"><span data-stu-id="9e790-185">Open [the necessary outgoing ports](app-insights-ip-addresses.md#outgoing-ports) in your server's firewall to allow Status Monitor to work.</span></span>

* <span data-ttu-id="9e790-186">Otevřete monitorování stavu a vyberte svou aplikaci v levém podokně.</span><span class="sxs-lookup"><span data-stu-id="9e790-186">Open Status Monitor and select your application on left pane.</span></span> <span data-ttu-id="9e790-187">Zkontrolujte, zda existují jakékoli zprávy diagnostiky pro tuto aplikaci v části „Konfigurace oznámení“:</span><span class="sxs-lookup"><span data-stu-id="9e790-187">Check if there are any diagnostics messages for this application in the "Configuration notifications" section:</span></span>

  ![Otevřete okno Výkon a zobrazte si žádost, dobu odezvy, závislosti a další data.](./media/app-insights-monitor-performance-live-website-now/appinsights-status-monitor-diagnostics-message.png)
* <span data-ttu-id="9e790-189">Na serveru, pokud se zobrazí zpráva o „nedostatečných oprávněních“, zkuste následující postup:</span><span class="sxs-lookup"><span data-stu-id="9e790-189">On the server, if you see a message about "insufficient permissions", try the following:</span></span>
  * <span data-ttu-id="9e790-190">Ve Správci služby IIS vyberte fond aplikací, otevřete položku **Upřesnit nastavení**, a v části **Model procesu** si povšimněte identity.</span><span class="sxs-lookup"><span data-stu-id="9e790-190">In IIS Manager, select your application pool, open **Advanced Settings**, and under **Process Model** note the identity.</span></span>
  * <span data-ttu-id="9e790-191">V ovládacích panelech správy počítače přidejte tuto identitu do skupiny uživatelů Sledování výkonu.</span><span class="sxs-lookup"><span data-stu-id="9e790-191">In Computer management control panel, add this identity to the Performance Monitor Users group.</span></span>
* <span data-ttu-id="9e790-192">Pokud máte na serveru nainstalovaný MMA/SCOM (System Center Operations Manager), může u některých verzí dojít ke konfliktu.</span><span class="sxs-lookup"><span data-stu-id="9e790-192">If you have MMA/SCOM (Systems Center Operations Manager) installed on your server, some versions can conflict.</span></span> <span data-ttu-id="9e790-193">Odinstalujte SCOM a sledování stavu a znovu nainstalujte nejnovější verze.</span><span class="sxs-lookup"><span data-stu-id="9e790-193">Uninstall both SCOM and Status Monitor, and re-install the latest versions.</span></span>
* <span data-ttu-id="9e790-194">Další informace najdete v tématu [Poradce při potížích][qna].</span><span class="sxs-lookup"><span data-stu-id="9e790-194">See [Troubleshooting][qna].</span></span>

## <a name="system-requirements"></a><span data-ttu-id="9e790-195">Systémové požadavky</span><span class="sxs-lookup"><span data-stu-id="9e790-195">System Requirements</span></span>
<span data-ttu-id="9e790-196">Podpora operačního systému pro sledování stavu Application Insights na serveru:</span><span class="sxs-lookup"><span data-stu-id="9e790-196">OS support for Application Insights Status Monitor on Server:</span></span>

* <span data-ttu-id="9e790-197">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="9e790-197">Windows Server 2008</span></span>
* <span data-ttu-id="9e790-198">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="9e790-198">Windows Server 2008 R2</span></span>
* <span data-ttu-id="9e790-199">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="9e790-199">Windows Server 2012</span></span>
* <span data-ttu-id="9e790-200">Windows server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="9e790-200">Windows server 2012 R2</span></span>
* <span data-ttu-id="9e790-201">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="9e790-201">Windows Server 2016</span></span>

<span data-ttu-id="9e790-202">s nejnovější aktualizací SP a rozhraním .NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="9e790-202">with latest SP and .NET Framework 4.5</span></span>

<span data-ttu-id="9e790-203">Na straně klienta: Windows 7, 8, 8.1 a 10, znovu s rozhraním .NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="9e790-203">On the client side: Windows 7, 8, 8.1 and 10, again with .NET Framework 4.5</span></span>

<span data-ttu-id="9e790-204">Podpora služby IIS je: IIS 7, 7.5, 8, 8.5 (je vyžadována služba IIS)</span><span class="sxs-lookup"><span data-stu-id="9e790-204">IIS support is: IIS 7, 7.5, 8, 8.5 (IIS is required)</span></span>

## <a name="automation-with-powershell"></a><span data-ttu-id="9e790-205">Automatizace v prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="9e790-205">Automation with PowerShell</span></span>
<span data-ttu-id="9e790-206">K zahájení a spuštění monitorování můžete na serveru služby IIS použít prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9e790-206">You can start and stop monitoring by using PowerShell on your IIS server.</span></span>

<span data-ttu-id="9e790-207">Nejdřív importujte modul Application Insights:</span><span class="sxs-lookup"><span data-stu-id="9e790-207">First import the Application Insights module:</span></span>

`Import-Module 'C:\Program Files\Microsoft Application Insights\Status Monitor\PowerShell\Microsoft.Diagnostics.Agent.StatusMonitor.PowerShell.dll'`

<span data-ttu-id="9e790-208">Zjistěte, které aplikace se monitorují:</span><span class="sxs-lookup"><span data-stu-id="9e790-208">Find out which apps are being monitored:</span></span>

`Get-ApplicationInsightsMonitoringStatus [-Name appName]`

* <span data-ttu-id="9e790-209">`-Name` (Volitelné) Název webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="9e790-209">`-Name` (Optional) The name of a web app.</span></span>
* <span data-ttu-id="9e790-210">Zobrazí sledování stavu Application Insights pro každou webovou aplikaci (nebo pojmenované aplikace) na tomto serveru služby IIS.</span><span class="sxs-lookup"><span data-stu-id="9e790-210">Displays the Application Insights monitoring status for each web app (or the named app) in this IIS server.</span></span>
* <span data-ttu-id="9e790-211">Vrátí `ApplicationInsightsApplication` pro každou aplikaci:</span><span class="sxs-lookup"><span data-stu-id="9e790-211">Returns `ApplicationInsightsApplication` for each app:</span></span>

  * <span data-ttu-id="9e790-212">`SdkState==EnabledAfterDeployment`: Aplikace je monitorována a byla instrumentována za běhu nástrojem Monitor stavu nebo rutinou `Start-ApplicationInsightsMonitoring`.</span><span class="sxs-lookup"><span data-stu-id="9e790-212">`SdkState==EnabledAfterDeployment`: App is being monitored, and was instrumented at run time, either by the Status Monitor tool, or by `Start-ApplicationInsightsMonitoring`.</span></span>
  * <span data-ttu-id="9e790-213">`SdkState==Disabled`: Aplikace není instrumentována pro Application Insights.</span><span class="sxs-lookup"><span data-stu-id="9e790-213">`SdkState==Disabled`: The app is not instrumented for Application Insights.</span></span> <span data-ttu-id="9e790-214">Buď nebyla nikdy instrumentována, nebo bylo zakázáno spuštění sledování pomocí nástroje Monitor stavu nebo pomocí `Stop-ApplicationInsightsMonitoring`.</span><span class="sxs-lookup"><span data-stu-id="9e790-214">Either it was never instrumented, or run-time monitoring was disabled with the Status Monitor tool or with `Stop-ApplicationInsightsMonitoring`.</span></span>
  * <span data-ttu-id="9e790-215">`SdkState==EnabledByCodeInstrumentation`: Aplikace byla instrumentována přidáním sady SDK do zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="9e790-215">`SdkState==EnabledByCodeInstrumentation`: The app was instrumented by adding the SDK to the source code.</span></span> <span data-ttu-id="9e790-216">Její SDK nelze aktualizovat ani zastavit.</span><span class="sxs-lookup"><span data-stu-id="9e790-216">Its SDK cannot be updated or stopped.</span></span>
  * <span data-ttu-id="9e790-217">`SdkVersion` zobrazuje verzi používanou k monitorování této aplikace.</span><span class="sxs-lookup"><span data-stu-id="9e790-217">`SdkVersion` shows the version in use for monitoring this app.</span></span>
  * <span data-ttu-id="9e790-218">`LatestAvailableSdkVersion` zobrazuje aktuálně dostupnou verzi v galerii NuGet.</span><span class="sxs-lookup"><span data-stu-id="9e790-218">`LatestAvailableSdkVersion`shows the version currently available on the NuGet gallery.</span></span> <span data-ttu-id="9e790-219">Chcete-li upgradovat aplikaci na tuto verzi, použijte `Update-ApplicationInsightsMonitoring`.</span><span class="sxs-lookup"><span data-stu-id="9e790-219">To upgrade the app to this version, use `Update-ApplicationInsightsMonitoring`.</span></span>

`Start-ApplicationInsightsMonitoring -Name appName -InstrumentationKey 00000000-000-000-000-0000000`

* <span data-ttu-id="9e790-220">`-Name` Název aplikace v IIS.</span><span class="sxs-lookup"><span data-stu-id="9e790-220">`-Name` The name of the app in IIS</span></span>
* <span data-ttu-id="9e790-221">`-InstrumentationKey` Ikey prostředku Application Insights, kde se mají zobrazovat výsledky.</span><span class="sxs-lookup"><span data-stu-id="9e790-221">`-InstrumentationKey` The ikey of the Application Insights resource where you want the results to be displayed.</span></span>
* <span data-ttu-id="9e790-222">Tato rutina ovlivní pouze aplikace, které již nejsou instrumentovány – to znamená, SdkState==NotInstrumented.</span><span class="sxs-lookup"><span data-stu-id="9e790-222">This cmdlet only affects apps that are not already instrumented - that is, SdkState==NotInstrumented.</span></span>

    <span data-ttu-id="9e790-223">Rutina neovlivní již instrumentované aplikace.</span><span class="sxs-lookup"><span data-stu-id="9e790-223">The cmdlet does not affect an app that is already instrumented.</span></span> <span data-ttu-id="9e790-224">Nezáleží na tom, jestli aplikace byla instrumentovaná v okamžiku sestavení přidáním sady SDK do kódu nebo v době běhu předchozím použitím této rutiny.</span><span class="sxs-lookup"><span data-stu-id="9e790-224">It does not matter whether the app was instrumented at build time by adding the SDK to the code, or at run time by a previous use of this cmdlet.</span></span>

    <span data-ttu-id="9e790-225">Verze sady SDK používaná k instrumentaci aplikace je verze, která byla naposledy stažena do tohoto serveru.</span><span class="sxs-lookup"><span data-stu-id="9e790-225">The SDK version used to instrument the app is the version that was most recently downloaded to this server.</span></span>

    <span data-ttu-id="9e790-226">Chcete-li stáhnout poslední verzi, použijte příkaz Update-ApplicationInsightsVersion.</span><span class="sxs-lookup"><span data-stu-id="9e790-226">To download the latest version, use Update-ApplicationInsightsVersion.</span></span>
* <span data-ttu-id="9e790-227">V případě úspěchu vrátí `ApplicationInsightsApplication`.</span><span class="sxs-lookup"><span data-stu-id="9e790-227">Returns `ApplicationInsightsApplication` on success.</span></span> <span data-ttu-id="9e790-228">Pokud se nezdaří, zaprotokoluje trasování do stderr.</span><span class="sxs-lookup"><span data-stu-id="9e790-228">If it fails, it logs a trace to stderr.</span></span>

          Name                      : Default Web Site/WebApp1
          InstrumentationKey        : 00000000-0000-0000-0000-000000000000
          ProfilerState             : ApplicationInsights
          SdkState                  : EnabledAfterDeployment
          SdkVersion                : 1.2.1
          LatestAvailableSdkVersion : 1.2.3

`Stop-ApplicationInsightsMonitoring [-Name appName | -All]`

* <span data-ttu-id="9e790-229">`-Name` Název aplikace v IIS.</span><span class="sxs-lookup"><span data-stu-id="9e790-229">`-Name` The name of an app in IIS</span></span>
* <span data-ttu-id="9e790-230">`-All` Zastaví monitorování všech aplikací v tomto serveru IIS, pro který platí, že `SdkState==EnabledAfterDeployment`.</span><span class="sxs-lookup"><span data-stu-id="9e790-230">`-All` Stops monitoring all apps in this IIS server for which `SdkState==EnabledAfterDeployment`</span></span>
* <span data-ttu-id="9e790-231">Zastaví monitorování zadané aplikace a odebere instrumentace.</span><span class="sxs-lookup"><span data-stu-id="9e790-231">Stops monitoring the specified apps and removes instrumentation.</span></span> <span data-ttu-id="9e790-232">Pracuje pouze pro aplikace, které byly instrumentovány v době běhu pomocí nástroje pro monitorování stavu nebo příkazu Start-ApplicationInsightsApplication.</span><span class="sxs-lookup"><span data-stu-id="9e790-232">It only works for apps that have been instrumented at run-time using the Status Monitoring tool or Start-ApplicationInsightsApplication.</span></span> <span data-ttu-id="9e790-233">(`SdkState==EnabledAfterDeployment`)</span><span class="sxs-lookup"><span data-stu-id="9e790-233">(`SdkState==EnabledAfterDeployment`)</span></span>
* <span data-ttu-id="9e790-234">Vrátí ApplicationInsightsApplication.</span><span class="sxs-lookup"><span data-stu-id="9e790-234">Returns ApplicationInsightsApplication.</span></span>

<span data-ttu-id="9e790-235">`Update-ApplicationInsightsMonitoring -Name appName [-InstrumentationKey "0000000-0000-000-000-0000"`]</span><span class="sxs-lookup"><span data-stu-id="9e790-235">`Update-ApplicationInsightsMonitoring -Name appName [-InstrumentationKey "0000000-0000-000-000-0000"`]</span></span>

* <span data-ttu-id="9e790-236">`-Name`: Název webové aplikace v IIS.</span><span class="sxs-lookup"><span data-stu-id="9e790-236">`-Name`: The name of a web app in IIS.</span></span>
* <span data-ttu-id="9e790-237">`-InstrumentationKey` (Volitelné) Tuto položku použijte ke změně prostředku, na kterou se telemetrie aplikace odesílá.</span><span class="sxs-lookup"><span data-stu-id="9e790-237">`-InstrumentationKey` (Optional.) Use this to change the resource to which the app's telemetry is sent.</span></span>
* <span data-ttu-id="9e790-238">Tato rutina:</span><span class="sxs-lookup"><span data-stu-id="9e790-238">This cmdlet:</span></span>
  * <span data-ttu-id="9e790-239">Upgrady pojmenované aplikace na verzi sady SDK naposledy stažené v tomto počítači.</span><span class="sxs-lookup"><span data-stu-id="9e790-239">Upgrades the named app to the version of the SDK most recently downloaded to this machine.</span></span> <span data-ttu-id="9e790-240">(Funguje pouze v případě `SdkState==EnabledAfterDeployment`)</span><span class="sxs-lookup"><span data-stu-id="9e790-240">(Only works if `SdkState==EnabledAfterDeployment`)</span></span>
  * <span data-ttu-id="9e790-241">Pokud jste zadali kód instrumentace, pojmenovaná aplikace se překonfiguruje na odeslání telemetrie do prostředku s tímto klíčem.</span><span class="sxs-lookup"><span data-stu-id="9e790-241">If you provide an instrumentation key, the named app is reconfigured to send telemetry to the resource with that key.</span></span> <span data-ttu-id="9e790-242">(Funguje v případě `SdkState != Disabled`)</span><span class="sxs-lookup"><span data-stu-id="9e790-242">(Works if `SdkState != Disabled`)</span></span>

`Update-ApplicationInsightsVersion`

* <span data-ttu-id="9e790-243">Stáhne nejnovější Application Insights SDK na server.</span><span class="sxs-lookup"><span data-stu-id="9e790-243">Downloads the latest Application Insights SDK to the server.</span></span>

## <span data-ttu-id="9e790-244"><a name="questions"></a>Dotazy týkající se Monitorování stavu</span><span class="sxs-lookup"><span data-stu-id="9e790-244"><a name="questions"></a>Questions about Status Monitor</span></span>

### <a name="what-is-status-monitor"></a><span data-ttu-id="9e790-245">Co je Monitorování stavu?</span><span class="sxs-lookup"><span data-stu-id="9e790-245">What is Status Monitor?</span></span>

<span data-ttu-id="9e790-246">Desktopová aplikace, kterou instalujete s webovým serverem IIS.</span><span class="sxs-lookup"><span data-stu-id="9e790-246">A desktop application that you install in your IIS web server.</span></span> <span data-ttu-id="9e790-247">Pomáhá provádět instrumentaci a konfiguraci webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="9e790-247">It helps you instrument and configure web apps.</span></span> 

### <a name="when-do-i-use-status-monitor"></a><span data-ttu-id="9e790-248">Kdy použít Monitorování stavu?</span><span class="sxs-lookup"><span data-stu-id="9e790-248">When do I use Status Monitor?</span></span>

* <span data-ttu-id="9e790-249">Při instrumentaci libovolné webové aplikace, která běží na serveru IIS, i když je už spuštěná.</span><span class="sxs-lookup"><span data-stu-id="9e790-249">To instrument any web app that is running on your IIS server - even if it is already running.</span></span>
* <span data-ttu-id="9e790-250">Při povolení další telemetrie pro webové aplikace, které byly [vytvořené pomocí sady Application Insights SDK](app-insights-asp-net.md), v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="9e790-250">To enable additional telemetry for web apps that have been [built with the Application Insights SDK](app-insights-asp-net.md) at compile time.</span></span> 

### <a name="can-i-close-it-after-it-runs"></a><span data-ttu-id="9e790-251">Můžu ji po spuštění zavřít?</span><span class="sxs-lookup"><span data-stu-id="9e790-251">Can I close it after it runs?</span></span>

<span data-ttu-id="9e790-252">Ano.</span><span class="sxs-lookup"><span data-stu-id="9e790-252">Yes.</span></span> <span data-ttu-id="9e790-253">Poté, co se provedla instrumentaci vybraných webových stránek, můžete ji zavřít.</span><span class="sxs-lookup"><span data-stu-id="9e790-253">After it has instrumented the websites you select, you can close it.</span></span>

<span data-ttu-id="9e790-254">Sama o sobě telemetrii neshromažďuje.</span><span class="sxs-lookup"><span data-stu-id="9e790-254">It doesn't collect telemetry by itself.</span></span> <span data-ttu-id="9e790-255">Pouze nakonfiguruje webové aplikace a nastaví některá oprávnění.</span><span class="sxs-lookup"><span data-stu-id="9e790-255">It just configures the web apps and sets some permissions.</span></span>

### <a name="what-does-status-monitor-do"></a><span data-ttu-id="9e790-256">K čemu Monitorování stavu slouží?</span><span class="sxs-lookup"><span data-stu-id="9e790-256">What does Status Monitor do?</span></span>

<span data-ttu-id="9e790-257">Když vyberete webovou aplikaci pro instrumentaci pomocí Monitorování stavu:</span><span class="sxs-lookup"><span data-stu-id="9e790-257">When you select a web app for Status Monitor to instrument:</span></span>

* <span data-ttu-id="9e790-258">Stáhne a umístí sestavení Application Insights a soubor .config do složky binárních souborů webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="9e790-258">Downloads and places the Application Insights assemblies and .config file in the web app's binaries folder.</span></span>
* <span data-ttu-id="9e790-259">Upraví soubor `web.config` přidáním modulu sledování HTTP pro Application Insights.</span><span class="sxs-lookup"><span data-stu-id="9e790-259">Modifies `web.config` to add the Application Insights HTTP tracking module.</span></span>
* <span data-ttu-id="9e790-260">Povolí profilaci CLR shromažďovat volání závislostí.</span><span class="sxs-lookup"><span data-stu-id="9e790-260">Enables CLR profiling to collect dependency calls.</span></span>

### <a name="do-i-need-to-run-status-monitor-whenever-i-update-the-app"></a><span data-ttu-id="9e790-261">Je potřeba spustit Monitorování stavu při každé aktualizaci aplikace?</span><span class="sxs-lookup"><span data-stu-id="9e790-261">Do I need to run Status Monitor whenever I update the app?</span></span>

<span data-ttu-id="9e790-262">Ne, pokud provádíte opakované nasazení postupně.</span><span class="sxs-lookup"><span data-stu-id="9e790-262">Not if you redeploy incrementally.</span></span> 

<span data-ttu-id="9e790-263">Pokud při procesu publikování vyberete možnost Odstranit stávající soubory, bude potřeba konfigurovat Application Insights opakovaným spuštěním Monitorování stavu.</span><span class="sxs-lookup"><span data-stu-id="9e790-263">If you select the 'delete existing files' option in the publish process, you would need to re-run Status Monitor to configure Application Insights.</span></span>

### <a name="what-telemetry-is-collected"></a><span data-ttu-id="9e790-264">Jaké telemetrická data se shromažďují?</span><span class="sxs-lookup"><span data-stu-id="9e790-264">What telemetry is collected?</span></span>

<span data-ttu-id="9e790-265">Pro aplikace instrumentované pouze za běhu pomocí Monitorování stavu:</span><span class="sxs-lookup"><span data-stu-id="9e790-265">For applications that you instrument only at run-time by using Status Monitor:</span></span>

* <span data-ttu-id="9e790-266">Požadavky HTTP</span><span class="sxs-lookup"><span data-stu-id="9e790-266">HTTP requests</span></span>
* <span data-ttu-id="9e790-267">Volání závislostí</span><span class="sxs-lookup"><span data-stu-id="9e790-267">Calls to dependencies</span></span>
* <span data-ttu-id="9e790-268">Výjimky</span><span class="sxs-lookup"><span data-stu-id="9e790-268">Exceptions</span></span>
* <span data-ttu-id="9e790-269">Čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="9e790-269">Performance counters</span></span>

<span data-ttu-id="9e790-270">Pro aplikace již instrumentované v době kompilace:</span><span class="sxs-lookup"><span data-stu-id="9e790-270">For applications already instrumented at compile time:</span></span>

 * <span data-ttu-id="9e790-271">Čítače procesů</span><span class="sxs-lookup"><span data-stu-id="9e790-271">Process counters.</span></span>
 * <span data-ttu-id="9e790-272">Volání závislostí (.NET 4.5); návratové hodnoty ve voláních závislostí (.NET 4.6)</span><span class="sxs-lookup"><span data-stu-id="9e790-272">Dependency calls (.NET 4.5); return values in dependency calls (.NET 4.6).</span></span>
 * <span data-ttu-id="9e790-273">Hodnoty trasování zásobníku výjimek</span><span class="sxs-lookup"><span data-stu-id="9e790-273">Exception stack trace values.</span></span>

[<span data-ttu-id="9e790-274">Další informace</span><span class="sxs-lookup"><span data-stu-id="9e790-274">Learn more</span></span>](http://apmtips.com/blog/2016/11/18/how-application-insights-status-monitor-not-monitors-dependencies/)

## <a name="video"></a><span data-ttu-id="9e790-275">Video</span><span class="sxs-lookup"><span data-stu-id="9e790-275">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <span data-ttu-id="9e790-276"><a name="next"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="9e790-276"><a name="next"></a>Next steps</span></span>

<span data-ttu-id="9e790-277">Zobrazení telemetrických dat:</span><span class="sxs-lookup"><span data-stu-id="9e790-277">View your telemetry:</span></span>

* <span data-ttu-id="9e790-278">[Zkoumání metrik](app-insights-metrics-explorer.md) pro monitorování výkonu a využití</span><span class="sxs-lookup"><span data-stu-id="9e790-278">[Explore metrics](app-insights-metrics-explorer.md) to monitor performance and usage</span></span>
* <span data-ttu-id="9e790-279">[Prohledávání událostí a protokolů][diagnostic] pro diagnostiku problémů</span><span class="sxs-lookup"><span data-stu-id="9e790-279">[Search events and logs][diagnostic] to diagnose problems</span></span>
* <span data-ttu-id="9e790-280">[Analýzy](app-insights-analytics.md) pro pokročilejší dotazy</span><span class="sxs-lookup"><span data-stu-id="9e790-280">[Analytics](app-insights-analytics.md) for more advanced queries</span></span>
* [<span data-ttu-id="9e790-281">Vytváření řídicích panelů</span><span class="sxs-lookup"><span data-stu-id="9e790-281">Create dashboards</span></span>](app-insights-dashboards.md)

<span data-ttu-id="9e790-282">Přidání další telemetrie:</span><span class="sxs-lookup"><span data-stu-id="9e790-282">Add more telemetry:</span></span>

* <span data-ttu-id="9e790-283">[Vytvoření webových testů][availability] a ověření, jestli web zůstává živý.</span><span class="sxs-lookup"><span data-stu-id="9e790-283">[Create web tests][availability] to make sure your site stays live.</span></span>
* <span data-ttu-id="9e790-284">[Přidání telemetrie webového klienta][usage] pro zobrazení výjimek z kódu webové stránky a umožnění vložení trasovacích volání.</span><span class="sxs-lookup"><span data-stu-id="9e790-284">[Add web client telemetry][usage] to see exceptions from web page code and to let you insert trace calls.</span></span>
* <span data-ttu-id="9e790-285">[Přidání sady Application Insights SDK do kódu][greenbrown] tak, abyste mohli vložit volání trasování a protokolování</span><span class="sxs-lookup"><span data-stu-id="9e790-285">[Add Application Insights SDK to your code][greenbrown] so that you can insert trace and log calls</span></span>

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[availability]: app-insights-monitor-web-app-availability.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-asp-net.md
[qna]: app-insights-troubleshoot-faq.md
[roles]: app-insights-resources-roles-access-control.md
[usage]: app-insights-javascript.md
