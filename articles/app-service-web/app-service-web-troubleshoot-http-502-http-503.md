---
title: "Opravte 502 Chybná brána, 503 Služba nedostupná chyby | Microsoft Docs"
description: "Vyřešte potíže Chybná brána 502 a 503 Služba není k dispozici chyby ve vaší webové aplikace hostované v Azure App Service."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: top-support-issue
keywords: "502 Chybná brána 503 Služba nedostupná, chyby 503, chyby 502"
ms.assetid: 51cd331a-a3fa-438f-90ef-385e755e50d5
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2016
ms.author: cephalin
ms.openlocfilehash: 397a6aaf7dc27adfa0fc0e722b8a2be5cc1d75f0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-http-errors-of-502-bad-gateway-and-503-service-unavailable-in-your-azure-web-apps"></a><span data-ttu-id="fcdb8-104">Řešení chyb HTTP "502 Chybná brána" a "503 Služba není k dispozici" ve službě Azure web apps</span><span class="sxs-lookup"><span data-stu-id="fcdb8-104">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>
<span data-ttu-id="fcdb8-105">"502 Chybná brána" a "503 Služba nedostupná" jsou běžné chyby ve vaší webové aplikace hostované v [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="fcdb8-105">"502 bad gateway" and "503 service unavailable" are common errors in your web app hosted in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="fcdb8-106">Tento článek vám pomůže vyřešit tyto chyby.</span><span class="sxs-lookup"><span data-stu-id="fcdb8-106">This article helps you troubleshoot these errors.</span></span>

<span data-ttu-id="fcdb8-107">Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na Azure odborníky na [MSDN Azure a fóra Stack Overflow](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="fcdb8-107">If you need more help at any point in this article, you can contact the Azure experts on [the MSDN Azure and the Stack Overflow forums](https://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="fcdb8-108">Alternativně můžete také soubor incidentu podpory Azure.</span><span class="sxs-lookup"><span data-stu-id="fcdb8-108">Alternatively, you can also file an Azure support incident.</span></span> <span data-ttu-id="fcdb8-109">Přejděte na [podporu Azure lokality](https://azure.microsoft.com/support/options/) a klikněte na **získat podporu**.</span><span class="sxs-lookup"><span data-stu-id="fcdb8-109">Go to the [Azure Support site](https://azure.microsoft.com/support/options/) and click on **Get Support**.</span></span>

## <a name="symptom"></a><span data-ttu-id="fcdb8-110">Příznaky</span><span class="sxs-lookup"><span data-stu-id="fcdb8-110">Symptom</span></span>
<span data-ttu-id="fcdb8-111">Když přejdete do webové aplikace, vrátí HTTP "502 Chybná brána" Chyba nebo HTTP Chyba "503 Služba není k dispozici".</span><span class="sxs-lookup"><span data-stu-id="fcdb8-111">When you browse to the web app, it returns a HTTP "502 Bad Gateway" error or a HTTP "503 Service Unavailable" error.</span></span>

## <a name="cause"></a><span data-ttu-id="fcdb8-112">Příčina</span><span class="sxs-lookup"><span data-stu-id="fcdb8-112">Cause</span></span>
<span data-ttu-id="fcdb8-113">Tento problém je často způsoben problémy na úrovni aplikací, například:</span><span class="sxs-lookup"><span data-stu-id="fcdb8-113">This problem is often caused by application level issues, such as:</span></span>

* <span data-ttu-id="fcdb8-114">požadavky trvá příliš dlouho</span><span class="sxs-lookup"><span data-stu-id="fcdb8-114">requests taking a long time</span></span>
* <span data-ttu-id="fcdb8-115">aplikace pomocí vysoké paměti nebo procesoru</span><span class="sxs-lookup"><span data-stu-id="fcdb8-115">application using high memory/CPU</span></span>
* <span data-ttu-id="fcdb8-116">aplikace chybám kvůli výjimce.</span><span class="sxs-lookup"><span data-stu-id="fcdb8-116">application crashing due to an exception.</span></span>

## <a name="troubleshooting-steps-to-solve-502-bad-gateway-and-503-service-unavailable-errors"></a><span data-ttu-id="fcdb8-117">Řešení potíží s kroky pro řešení "502 Chybná brána" a "503 Služba nedostupná" chyb</span><span class="sxs-lookup"><span data-stu-id="fcdb8-117">Troubleshooting steps to solve "502 bad gateway" and "503 service unavailable" errors</span></span>
<span data-ttu-id="fcdb8-118">Řešení potíží s jde rozdělit na tři samostatné úkoly v sekvenčním pořadí:</span><span class="sxs-lookup"><span data-stu-id="fcdb8-118">Troubleshooting can be divided into three distinct tasks, in sequential order:</span></span>

1. [<span data-ttu-id="fcdb8-119">Sledovat a monitorovat chování aplikace</span><span class="sxs-lookup"><span data-stu-id="fcdb8-119">Observe and monitor application behavior</span></span>](#observe)
2. [<span data-ttu-id="fcdb8-120">Shromažďování dat</span><span class="sxs-lookup"><span data-stu-id="fcdb8-120">Collect data</span></span>](#collect)
3. [<span data-ttu-id="fcdb8-121">Zmírnění problém</span><span class="sxs-lookup"><span data-stu-id="fcdb8-121">Mitigate the issue</span></span>](#mitigate)

<span data-ttu-id="fcdb8-122">[App Service Web Apps](/services/app-service/web/) nabízí různé možnosti v každém kroku.</span><span class="sxs-lookup"><span data-stu-id="fcdb8-122">[App Service Web Apps](/services/app-service/web/) gives you various options at each step.</span></span>

<a name="observe" />

### <a name="1-observe-and-monitor-application-behavior"></a><span data-ttu-id="fcdb8-123">1. Sledovat a monitorovat chování aplikace</span><span class="sxs-lookup"><span data-stu-id="fcdb8-123">1. Observe and monitor application behavior</span></span>
#### <a name="track-service-health"></a><span data-ttu-id="fcdb8-124">Sledování stavu služby</span><span class="sxs-lookup"><span data-stu-id="fcdb8-124">Track Service health</span></span>
<span data-ttu-id="fcdb8-125">Microsoft Azure publicizes pokaždé, když je služba došlo k přerušení nebo výkonu snížení.</span><span class="sxs-lookup"><span data-stu-id="fcdb8-125">Microsoft Azure publicizes each time there is a service interruption or performance degradation.</span></span> <span data-ttu-id="fcdb8-126">Stav služby můžete sledovat na [portálu Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="fcdb8-126">You can track the health of the service on the [Azure Portal](https://portal.azure.com/).</span></span> <span data-ttu-id="fcdb8-127">Další informace najdete v tématu [sledovat stav služeb](../monitoring-and-diagnostics/insights-service-health.md).</span><span class="sxs-lookup"><span data-stu-id="fcdb8-127">For more information, see [Track service health](../monitoring-and-diagnostics/insights-service-health.md).</span></span>

#### <a name="monitor-your-web-app"></a><span data-ttu-id="fcdb8-128">Monitorování webové aplikace</span><span class="sxs-lookup"><span data-stu-id="fcdb8-128">Monitor your web app</span></span>
<span data-ttu-id="fcdb8-129">Tato možnost vám umožňuje zjistit, pokud se žádné problémy s vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="fcdb8-129">This option enables you to find out if your application is having any issues.</span></span> <span data-ttu-id="fcdb8-130">V okně vaší webové aplikace, klikněte **požadavky a chyby** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="fcdb8-130">In your web app’s blade, click the **Requests and errors** tile.</span></span> <span data-ttu-id="fcdb8-131">**Metrika** okno se zobrazí všechny metriky můžete přidat.</span><span class="sxs-lookup"><span data-stu-id="fcdb8-131">The **Metric** blade will show you all the metrics you can add.</span></span>

<span data-ttu-id="fcdb8-132">Některé z metriky, které můžete chtít monitorování pro webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="fcdb8-132">Some of the metrics that you might want to monitor for your web app are</span></span>

* <span data-ttu-id="fcdb8-133">Průměrná paměti pracovní sady</span><span class="sxs-lookup"><span data-stu-id="fcdb8-133">Average memory working set</span></span>
* <span data-ttu-id="fcdb8-134">Průměrná doba odezvy</span><span class="sxs-lookup"><span data-stu-id="fcdb8-134">Average response time</span></span>
* <span data-ttu-id="fcdb8-135">Čas procesoru</span><span class="sxs-lookup"><span data-stu-id="fcdb8-135">CPU time</span></span>
* <span data-ttu-id="fcdb8-136">Paměť pracovní sady</span><span class="sxs-lookup"><span data-stu-id="fcdb8-136">Memory working set</span></span>
* <span data-ttu-id="fcdb8-137">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fcdb8-137">Requests</span></span>

![monitorování webové aplikace k řešení chyb HTTP 502 Chybná brána a 503 Služba nedostupná](./media/app-service-web-troubleshoot-HTTP-502-503/1-monitor-metrics.png)

<span data-ttu-id="fcdb8-139">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="fcdb8-139">For more information, see:</span></span>

* [<span data-ttu-id="fcdb8-140">Monitorování webové aplikace v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="fcdb8-140">Monitor Web Apps in Azure App Service</span></span>](web-sites-monitor.md)
* [<span data-ttu-id="fcdb8-141">Zobrazování oznámení o výstrahách</span><span class="sxs-lookup"><span data-stu-id="fcdb8-141">Receive alert notifications</span></span>](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

<a name="collect" />

### <a name="2-collect-data"></a><span data-ttu-id="fcdb8-142">2. Shromažďování dat</span><span class="sxs-lookup"><span data-stu-id="fcdb8-142">2. Collect data</span></span>
#### <a name="use-the-azure-app-service-support-portal"></a><span data-ttu-id="fcdb8-143">Použití portálu podporu služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="fcdb8-143">Use the Azure App Service Support Portal</span></span>
<span data-ttu-id="fcdb8-144">Web Apps poskytuje možnost řešení problémů s prohlížením HTTP protokoly, protokoly událostí, výpisy procesů a další související s vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="fcdb8-144">Web Apps provides you with the ability to troubleshoot issues related to your web app by looking at HTTP logs, event logs, process dumps, and more.</span></span> <span data-ttu-id="fcdb8-145">Dostanete tyto informace pomocí náš portál podpory v **http://&lt;název aplikace >.scm.azurewebsites.net/Support**</span><span class="sxs-lookup"><span data-stu-id="fcdb8-145">You can access all this information using our Support portal at **http://&lt;your app name>.scm.azurewebsites.net/Support**</span></span>

<span data-ttu-id="fcdb8-146">Portálu pro podporu Azure App Service nabízí tři samostatné karty pro podporu tři kroky běžné scénáře řešení potíží:</span><span class="sxs-lookup"><span data-stu-id="fcdb8-146">The Azure App Service Support portal provides you with three separate tabs to support the three steps of a common troubleshooting scenario:</span></span>

1. <span data-ttu-id="fcdb8-147">Sledovat aktuální chování</span><span class="sxs-lookup"><span data-stu-id="fcdb8-147">Observe current behavior</span></span>
2. <span data-ttu-id="fcdb8-148">Analýza shromažďování diagnostické informace a spuštěním předdefinované analyzátory</span><span class="sxs-lookup"><span data-stu-id="fcdb8-148">Analyze by collecting diagnostics information and running the built-in analyzers</span></span>
3. <span data-ttu-id="fcdb8-149">Zmírnění</span><span class="sxs-lookup"><span data-stu-id="fcdb8-149">Mitigate</span></span>

<span data-ttu-id="fcdb8-150">Pokud tento problém se děje nyní, klikněte na tlačítko **analyzovat** > **diagnostiky** > **diagnostikovat teď** k vytvoření relace diagnostiky, které bude shromažďovat protokoly HTTP, protokoly Prohlížeče událostí, paměti, že výpisy, protokoly chyb PHP a PHP, zpracování sestavy.</span><span class="sxs-lookup"><span data-stu-id="fcdb8-150">If the issue is happening right now, click **Analyze** > **Diagnostics** > **Diagnose Now** to create a diagnostic session for you, which will collect HTTP logs, event viewer logs, memory dumps, PHP error logs and PHP process report.</span></span>

<span data-ttu-id="fcdb8-151">Jakmile data jsou shromažďována, bude také spustit analýzu na datech a poskytnout sestavu ve formátu HTML.</span><span class="sxs-lookup"><span data-stu-id="fcdb8-151">Once the data is collected, it will also run an analysis on the data and provide you with an HTML report.</span></span>

<span data-ttu-id="fcdb8-152">V případě, že chcete stáhnout data, ve výchozím nastavení, by je uložená ve složce D:\home\data\DaaS.</span><span class="sxs-lookup"><span data-stu-id="fcdb8-152">In case you want to download the data, by default, it would be stored in the D:\home\data\DaaS folder.</span></span>

<span data-ttu-id="fcdb8-153">Další informace o portálu podpora služby Azure App Service naleznete v tématu [nové aktualizace ke rozšíření lokality podporu pro weby sady Azure](https://azure.microsoft.com/blog/new-updates-to-support-site-extension-for-azure-websites).</span><span class="sxs-lookup"><span data-stu-id="fcdb8-153">For more information on the Azure App Service Support portal, see [New Updates to Support Site Extension for Azure Websites](https://azure.microsoft.com/blog/new-updates-to-support-site-extension-for-azure-websites).</span></span>

#### <a name="use-the-kudu-debug-console"></a><span data-ttu-id="fcdb8-154">Použít konzolu pro ladění modulu Kudu</span><span class="sxs-lookup"><span data-stu-id="fcdb8-154">Use the Kudu Debug Console</span></span>
<span data-ttu-id="fcdb8-155">Webové aplikace se dodává s konzolou pro ladění, který můžete použít pro ladění, prohlížení, nahrávání souborů a také koncové body JSON pro získání informací o vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="fcdb8-155">Web Apps comes with a debug console that you can use for debugging, exploring, uploading files, as well as JSON endpoints for getting information about your environment.</span></span> <span data-ttu-id="fcdb8-156">To se označuje jako *Kudu konzoly* nebo *řídicí panel SCM* pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="fcdb8-156">This is called the *Kudu Console* or the *SCM Dashboard* for your web app.</span></span>

<span data-ttu-id="fcdb8-157">Dostanete tento řídicí panel tak, že přejdete na odkaz **https://&lt;název aplikace >.scm.azurewebsites.net/**.</span><span class="sxs-lookup"><span data-stu-id="fcdb8-157">You can access this dashboard by going to the link **https://&lt;Your app name>.scm.azurewebsites.net/**.</span></span>

<span data-ttu-id="fcdb8-158">Některé možnosti, které Kudu poskytuje jsou:</span><span class="sxs-lookup"><span data-stu-id="fcdb8-158">Some of the things that Kudu provides are:</span></span>

* <span data-ttu-id="fcdb8-159">nastavení prostředí pro vaši aplikaci</span><span class="sxs-lookup"><span data-stu-id="fcdb8-159">environment settings for your application</span></span>
* <span data-ttu-id="fcdb8-160">datový proud protokolu</span><span class="sxs-lookup"><span data-stu-id="fcdb8-160">log stream</span></span>
* <span data-ttu-id="fcdb8-161">diagnostické výpis</span><span class="sxs-lookup"><span data-stu-id="fcdb8-161">diagnostic dump</span></span>
* <span data-ttu-id="fcdb8-162">ladění konzoly, ve kterém můžete spouštět rutiny prostředí Powershell a základních příkazů DOS.</span><span class="sxs-lookup"><span data-stu-id="fcdb8-162">debug console in which you can run Powershell cmdlets and basic DOS commands.</span></span>

<span data-ttu-id="fcdb8-163">Další užitečné funkce Kudu je, že v případě, že aplikace je vyvolání first chance výjimek, můžete použít Kudu a vypíše nástroj SysInternals Procdump vytvořit paměti.</span><span class="sxs-lookup"><span data-stu-id="fcdb8-163">Another useful feature of Kudu is that, in case your application is throwing first-chance exceptions, you can use Kudu and the SysInternals tool Procdump to create memory dumps.</span></span> <span data-ttu-id="fcdb8-164">Tyto výpisy paměti jsou snímky procesu a často může pomoci při odstraňování složitějších problémů s vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="fcdb8-164">These memory dumps are snapshots of the process and can often help you troubleshoot more complicated issues with your web app.</span></span>

<span data-ttu-id="fcdb8-165">Další informace o funkcích, které jsou k dispozici v Kudu, najdete v části [online nástroje weby Azure, měli byste vědět o](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/).</span><span class="sxs-lookup"><span data-stu-id="fcdb8-165">For more information on features available in Kudu, see [Azure Websites online tools you should know about](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/).</span></span>

<a name="mitigate" />

### <a name="3-mitigate-the-issue"></a><span data-ttu-id="fcdb8-166">3. Zmírnění problém</span><span class="sxs-lookup"><span data-stu-id="fcdb8-166">3. Mitigate the issue</span></span>
#### <a name="scale-the-web-app"></a><span data-ttu-id="fcdb8-167">Škálování webové aplikace</span><span class="sxs-lookup"><span data-stu-id="fcdb8-167">Scale the web app</span></span>
<span data-ttu-id="fcdb8-168">Ve službě Azure App Service pro vyšší výkon a propustnost, můžete upravit škálování, ve kterém je spuštěná vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="fcdb8-168">In Azure App Service, for increased performance and throughput,  you can adjust the scale at which you are running your application.</span></span> <span data-ttu-id="fcdb8-169">Škálování webovou aplikaci zahrnuje dvě souvisejících akcích: Změna plánu služby App Service na používat vyšší cenová úroveň a konfigurace určitá nastavení po jste přepnuli do vyšší cenová úroveň.</span><span class="sxs-lookup"><span data-stu-id="fcdb8-169">Scaling up a web app involves two related actions: changing your App Service plan to a higher pricing tier, and configuring certain settings after you have switched to the higher pricing tier.</span></span>

<span data-ttu-id="fcdb8-170">Další informace o škálování najdete v tématu [škálování webové aplikace v Azure App Service](web-sites-scale.md).</span><span class="sxs-lookup"><span data-stu-id="fcdb8-170">For more information on scaling, see [Scale a web app in Azure App Service](web-sites-scale.md).</span></span>

<span data-ttu-id="fcdb8-171">Kromě toho můžete spustit aplikaci na více než jednu instanci.</span><span class="sxs-lookup"><span data-stu-id="fcdb8-171">Additionally, you can choose to run your application on more than one instance .</span></span> <span data-ttu-id="fcdb8-172">Pouze to poskytuje další možnost zpracování, ale také vám dává některé množství odolnost proti chybám.</span><span class="sxs-lookup"><span data-stu-id="fcdb8-172">This not only provides you with more processing capability, but also gives you some amount of fault tolerance.</span></span> <span data-ttu-id="fcdb8-173">Pokud proces přestane fungovat na jednu instanci, ostatní instance pokračovat, obsluhovat požadavky.</span><span class="sxs-lookup"><span data-stu-id="fcdb8-173">If the process goes down on one instance, the other instance will still continue serving requests.</span></span>

<span data-ttu-id="fcdb8-174">Můžete nastavit škálování ručně nebo automaticky.</span><span class="sxs-lookup"><span data-stu-id="fcdb8-174">You can set the scaling to be Manual or Automatic.</span></span>

#### <a name="use-autoheal"></a><span data-ttu-id="fcdb8-175">Pomocí funkce AutoHeal</span><span class="sxs-lookup"><span data-stu-id="fcdb8-175">Use AutoHeal</span></span>
<span data-ttu-id="fcdb8-176">Funkce AutoHeal recykluje pracovní proces pro vaši aplikaci na základě nastavení, které zvolíte (například změny konfigurace, požadavky, omezení na základě paměti nebo doba potřebná k provedení požadavku).</span><span class="sxs-lookup"><span data-stu-id="fcdb8-176">AutoHeal recycles the worker process for your app based on settings you choose (like configuration changes, requests, memory-based limits, or the time needed to execute a request).</span></span> <span data-ttu-id="fcdb8-177">Ve většině případů, recyklaci proces je nejrychlejší způsob, jak obnovit z problém.</span><span class="sxs-lookup"><span data-stu-id="fcdb8-177">Most of the time, recycle the process is the fastest way to recover from a problem.</span></span> <span data-ttu-id="fcdb8-178">I když můžete vždy restartování webové aplikace z přímo na portálu Azure, funkce AutoHeal bude provádět automaticky za vás.</span><span class="sxs-lookup"><span data-stu-id="fcdb8-178">Though you can always restart the web app from directly within the Azure Portal, AutoHeal will do it automatically for you.</span></span> <span data-ttu-id="fcdb8-179">Všechny, které musíte udělat, je přidat některé aktivační události v kořenovém souboru web.config pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="fcdb8-179">All you need to do is add some triggers in the root web.config for your web app.</span></span> <span data-ttu-id="fcdb8-180">Mějte na paměti, že by tato nastavení fungují v stejným způsobem, i v případě, že vaše aplikace není .net jeden.</span><span class="sxs-lookup"><span data-stu-id="fcdb8-180">Note that these settings would work in the same way even if your application is not a .Net one.</span></span>

<span data-ttu-id="fcdb8-181">Další informace najdete v tématu [automatické opravy weby Azure](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/).</span><span class="sxs-lookup"><span data-stu-id="fcdb8-181">For more information, see [Auto-Healing Azure Web Sites](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/).</span></span>

#### <a name="restart-the-web-app"></a><span data-ttu-id="fcdb8-182">Restartování webové aplikace</span><span class="sxs-lookup"><span data-stu-id="fcdb8-182">Restart the web app</span></span>
<span data-ttu-id="fcdb8-183">To je často nejjednodušší způsob, jak obnovit z jednorázových problémů.</span><span class="sxs-lookup"><span data-stu-id="fcdb8-183">This is often the simplest way to recover from one-time issues.</span></span> <span data-ttu-id="fcdb8-184">Na [portálu Azure](https://portal.azure.com/), v okně vaší webové aplikace, zobrazí se možnosti pro zastavení nebo restartování aplikace.</span><span class="sxs-lookup"><span data-stu-id="fcdb8-184">On the [Azure Portal](https://portal.azure.com/), on your web app’s blade, you have the options to stop or restart your app.</span></span>

 ![Restartujte aplikace k řešení chyb HTTP 502 Chybná brána a 503 Služba nedostupná](./media/app-service-web-troubleshoot-HTTP-502-503/2-restart.png)

<span data-ttu-id="fcdb8-186">Můžete také spravovat webové aplikace pomocí Azure Powershell.</span><span class="sxs-lookup"><span data-stu-id="fcdb8-186">You can also manage your web app using Azure Powershell.</span></span> <span data-ttu-id="fcdb8-187">Další informace najdete v tématu [Použití Azure PowerShellu s Azure Resource Managerem](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="fcdb8-187">For more information, see [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md).</span></span>

