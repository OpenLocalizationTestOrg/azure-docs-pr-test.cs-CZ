---
title: "aaaFix 502 Chybná brána, 503 Služba nedostupná chyby | Microsoft Docs"
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
ms.openlocfilehash: d9d8dcddaac930967a2e8d2bfd8cad09e6824c17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-http-errors-of-502-bad-gateway-and-503-service-unavailable-in-your-azure-web-apps"></a><span data-ttu-id="cc5db-104">Řešení chyb HTTP "502 Chybná brána" a "503 Služba není k dispozici" ve službě Azure web apps</span><span class="sxs-lookup"><span data-stu-id="cc5db-104">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>
<span data-ttu-id="cc5db-105">"502 Chybná brána" a "503 Služba nedostupná" jsou běžné chyby ve vaší webové aplikace hostované v [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="cc5db-105">"502 bad gateway" and "503 service unavailable" are common errors in your web app hosted in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="cc5db-106">Tento článek vám pomůže vyřešit tyto chyby.</span><span class="sxs-lookup"><span data-stu-id="cc5db-106">This article helps you troubleshoot these errors.</span></span>

<span data-ttu-id="cc5db-107">Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na hello Azure odborníky na [hello MSDN Azure a hello Stack Overflow fóra](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="cc5db-107">If you need more help at any point in this article, you can contact hello Azure experts on [hello MSDN Azure and hello Stack Overflow forums](https://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="cc5db-108">Alternativně můžete také soubor incidentu podpory Azure.</span><span class="sxs-lookup"><span data-stu-id="cc5db-108">Alternatively, you can also file an Azure support incident.</span></span> <span data-ttu-id="cc5db-109">Přejděte toohello [podporu Azure lokality](https://azure.microsoft.com/support/options/) a klikněte na **získat podporu**.</span><span class="sxs-lookup"><span data-stu-id="cc5db-109">Go toohello [Azure Support site](https://azure.microsoft.com/support/options/) and click on **Get Support**.</span></span>

## <a name="symptom"></a><span data-ttu-id="cc5db-110">Příznaky</span><span class="sxs-lookup"><span data-stu-id="cc5db-110">Symptom</span></span>
<span data-ttu-id="cc5db-111">Při procházení toohello webové aplikace, vrátí HTTP "502 Chybná brána" Chyba nebo HTTP Chyba "503 Služba není k dispozici".</span><span class="sxs-lookup"><span data-stu-id="cc5db-111">When you browse toohello web app, it returns a HTTP "502 Bad Gateway" error or a HTTP "503 Service Unavailable" error.</span></span>

## <a name="cause"></a><span data-ttu-id="cc5db-112">Příčina</span><span class="sxs-lookup"><span data-stu-id="cc5db-112">Cause</span></span>
<span data-ttu-id="cc5db-113">Tento problém je často způsoben problémy na úrovni aplikací, například:</span><span class="sxs-lookup"><span data-stu-id="cc5db-113">This problem is often caused by application level issues, such as:</span></span>

* <span data-ttu-id="cc5db-114">požadavky trvá příliš dlouho</span><span class="sxs-lookup"><span data-stu-id="cc5db-114">requests taking a long time</span></span>
* <span data-ttu-id="cc5db-115">aplikace pomocí vysoké paměti nebo procesoru</span><span class="sxs-lookup"><span data-stu-id="cc5db-115">application using high memory/CPU</span></span>
* <span data-ttu-id="cc5db-116">selhání kvůli výjimce tooan aplikace.</span><span class="sxs-lookup"><span data-stu-id="cc5db-116">application crashing due tooan exception.</span></span>

## <a name="troubleshooting-steps-toosolve-502-bad-gateway-and-503-service-unavailable-errors"></a><span data-ttu-id="cc5db-117">Řešení potíží s kroky toosolve "502 Chybná brána" a "503 Služba nedostupná" chyb</span><span class="sxs-lookup"><span data-stu-id="cc5db-117">Troubleshooting steps toosolve "502 bad gateway" and "503 service unavailable" errors</span></span>
<span data-ttu-id="cc5db-118">Řešení potíží s jde rozdělit na tři samostatné úkoly v sekvenčním pořadí:</span><span class="sxs-lookup"><span data-stu-id="cc5db-118">Troubleshooting can be divided into three distinct tasks, in sequential order:</span></span>

1. [<span data-ttu-id="cc5db-119">Sledovat a monitorovat chování aplikace</span><span class="sxs-lookup"><span data-stu-id="cc5db-119">Observe and monitor application behavior</span></span>](#observe)
2. [<span data-ttu-id="cc5db-120">Shromažďování dat</span><span class="sxs-lookup"><span data-stu-id="cc5db-120">Collect data</span></span>](#collect)
3. [<span data-ttu-id="cc5db-121">Zmírnění hello problém</span><span class="sxs-lookup"><span data-stu-id="cc5db-121">Mitigate hello issue</span></span>](#mitigate)

<span data-ttu-id="cc5db-122">[App Service Web Apps](/services/app-service/web/) nabízí různé možnosti v každém kroku.</span><span class="sxs-lookup"><span data-stu-id="cc5db-122">[App Service Web Apps](/services/app-service/web/) gives you various options at each step.</span></span>

<a name="observe" />

### <a name="1-observe-and-monitor-application-behavior"></a><span data-ttu-id="cc5db-123">1. Sledovat a monitorovat chování aplikace</span><span class="sxs-lookup"><span data-stu-id="cc5db-123">1. Observe and monitor application behavior</span></span>
#### <a name="track-service-health"></a><span data-ttu-id="cc5db-124">Sledování stavu služby</span><span class="sxs-lookup"><span data-stu-id="cc5db-124">Track Service health</span></span>
<span data-ttu-id="cc5db-125">Microsoft Azure publicizes pokaždé, když je služba došlo k přerušení nebo výkonu snížení.</span><span class="sxs-lookup"><span data-stu-id="cc5db-125">Microsoft Azure publicizes each time there is a service interruption or performance degradation.</span></span> <span data-ttu-id="cc5db-126">Stav hello hello služby můžete sledovat na hello [portálu Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="cc5db-126">You can track hello health of hello service on hello [Azure Portal](https://portal.azure.com/).</span></span> <span data-ttu-id="cc5db-127">Další informace najdete v tématu [sledovat stav služeb](../monitoring-and-diagnostics/insights-service-health.md).</span><span class="sxs-lookup"><span data-stu-id="cc5db-127">For more information, see [Track service health](../monitoring-and-diagnostics/insights-service-health.md).</span></span>

#### <a name="monitor-your-web-app"></a><span data-ttu-id="cc5db-128">Monitorování webové aplikace</span><span class="sxs-lookup"><span data-stu-id="cc5db-128">Monitor your web app</span></span>
<span data-ttu-id="cc5db-129">Tato možnost umožňuje toofind out, pokud vaše aplikace je žádné problémy s.</span><span class="sxs-lookup"><span data-stu-id="cc5db-129">This option enables you toofind out if your application is having any issues.</span></span> <span data-ttu-id="cc5db-130">V okně vaší webové aplikace, klikněte na tlačítko hello **požadavky a chyby** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="cc5db-130">In your web app’s blade, click hello **Requests and errors** tile.</span></span> <span data-ttu-id="cc5db-131">Hello **metrika** okno se zobrazí všechny metriky hello můžete přidat.</span><span class="sxs-lookup"><span data-stu-id="cc5db-131">hello **Metric** blade will show you all hello metrics you can add.</span></span>

<span data-ttu-id="cc5db-132">Některé hello metriky, můžete toomonitor pro vaši webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="cc5db-132">Some of hello metrics that you might want toomonitor for your web app are</span></span>

* <span data-ttu-id="cc5db-133">Průměrná paměti pracovní sady</span><span class="sxs-lookup"><span data-stu-id="cc5db-133">Average memory working set</span></span>
* <span data-ttu-id="cc5db-134">Průměrná doba odezvy</span><span class="sxs-lookup"><span data-stu-id="cc5db-134">Average response time</span></span>
* <span data-ttu-id="cc5db-135">Čas procesoru</span><span class="sxs-lookup"><span data-stu-id="cc5db-135">CPU time</span></span>
* <span data-ttu-id="cc5db-136">Paměť pracovní sady</span><span class="sxs-lookup"><span data-stu-id="cc5db-136">Memory working set</span></span>
* <span data-ttu-id="cc5db-137">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cc5db-137">Requests</span></span>

![monitorování webové aplikace k řešení chyb HTTP 502 Chybná brána a 503 Služba nedostupná](./media/app-service-web-troubleshoot-HTTP-502-503/1-monitor-metrics.png)

<span data-ttu-id="cc5db-139">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="cc5db-139">For more information, see:</span></span>

* [<span data-ttu-id="cc5db-140">Monitorování webové aplikace v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="cc5db-140">Monitor Web Apps in Azure App Service</span></span>](web-sites-monitor.md)
* [<span data-ttu-id="cc5db-141">Zobrazování oznámení o výstrahách</span><span class="sxs-lookup"><span data-stu-id="cc5db-141">Receive alert notifications</span></span>](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

<a name="collect" />

### <a name="2-collect-data"></a><span data-ttu-id="cc5db-142">2. Shromažďování dat</span><span class="sxs-lookup"><span data-stu-id="cc5db-142">2. Collect data</span></span>
#### <a name="use-hello-azure-app-service-support-portal"></a><span data-ttu-id="cc5db-143">Hello použití portálu Azure App Service podpory</span><span class="sxs-lookup"><span data-stu-id="cc5db-143">Use hello Azure App Service Support Portal</span></span>
<span data-ttu-id="cc5db-144">Web Apps poskytuje hello možnost tootroubleshoot problémy související tooyour webové aplikace prohlížením HTTP protokoly, protokoly událostí, výpisy procesů a další.</span><span class="sxs-lookup"><span data-stu-id="cc5db-144">Web Apps provides you with hello ability tootroubleshoot issues related tooyour web app by looking at HTTP logs, event logs, process dumps, and more.</span></span> <span data-ttu-id="cc5db-145">Dostanete tyto informace pomocí náš portál podpory v **http://&lt;název aplikace >.scm.azurewebsites.net/Support**</span><span class="sxs-lookup"><span data-stu-id="cc5db-145">You can access all this information using our Support portal at **http://&lt;your app name>.scm.azurewebsites.net/Support**</span></span>

<span data-ttu-id="cc5db-146">portál Hello podpora služby Azure App Service nabízí tři samostatné karty toosupport hello tři kroky běžné scénáře řešení potíží:</span><span class="sxs-lookup"><span data-stu-id="cc5db-146">hello Azure App Service Support portal provides you with three separate tabs toosupport hello three steps of a common troubleshooting scenario:</span></span>

1. <span data-ttu-id="cc5db-147">Sledovat aktuální chování</span><span class="sxs-lookup"><span data-stu-id="cc5db-147">Observe current behavior</span></span>
2. <span data-ttu-id="cc5db-148">Analýza shromažďování diagnostické informace a spuštěním hello předdefinované analyzátory</span><span class="sxs-lookup"><span data-stu-id="cc5db-148">Analyze by collecting diagnostics information and running hello built-in analyzers</span></span>
3. <span data-ttu-id="cc5db-149">Zmírnění</span><span class="sxs-lookup"><span data-stu-id="cc5db-149">Mitigate</span></span>

<span data-ttu-id="cc5db-150">Pokud problém hello se děje nyní, klikněte na **analyzovat** > **diagnostiky** > **diagnostikovat teď** toocreate diagnostické relaci, která bude shromažďovat HTTP zaznamená, protokoly Prohlížeče událostí, paměti, že výpisy, protokoly chyb PHP a PHP, zpracování sestavy.</span><span class="sxs-lookup"><span data-stu-id="cc5db-150">If hello issue is happening right now, click **Analyze** > **Diagnostics** > **Diagnose Now** toocreate a diagnostic session for you, which will collect HTTP logs, event viewer logs, memory dumps, PHP error logs and PHP process report.</span></span>

<span data-ttu-id="cc5db-151">Jakmile hello data jsou shromažďována, bude také spustit analýzu na hello dat a poskytnout sestavu ve formátu HTML.</span><span class="sxs-lookup"><span data-stu-id="cc5db-151">Once hello data is collected, it will also run an analysis on hello data and provide you with an HTML report.</span></span>

<span data-ttu-id="cc5db-152">V případě, že chcete toodownload hello data, ve výchozím nastavení, by je uložená ve složce D:\home\data\DaaS hello.</span><span class="sxs-lookup"><span data-stu-id="cc5db-152">In case you want toodownload hello data, by default, it would be stored in hello D:\home\data\DaaS folder.</span></span>

<span data-ttu-id="cc5db-153">Další informace o portálu hello podpora služby Azure App Service naleznete v části [tooSupport nové aktualizace rozšíření lokality pro weby sady Azure](https://azure.microsoft.com/blog/new-updates-to-support-site-extension-for-azure-websites).</span><span class="sxs-lookup"><span data-stu-id="cc5db-153">For more information on hello Azure App Service Support portal, see [New Updates tooSupport Site Extension for Azure Websites](https://azure.microsoft.com/blog/new-updates-to-support-site-extension-for-azure-websites).</span></span>

#### <a name="use-hello-kudu-debug-console"></a><span data-ttu-id="cc5db-154">Použití hello konzoly ladění modulu Kudu</span><span class="sxs-lookup"><span data-stu-id="cc5db-154">Use hello Kudu Debug Console</span></span>
<span data-ttu-id="cc5db-155">Webové aplikace se dodává s konzolou pro ladění, který můžete použít pro ladění, prohlížení, nahrávání souborů a také koncové body JSON pro získání informací o vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="cc5db-155">Web Apps comes with a debug console that you can use for debugging, exploring, uploading files, as well as JSON endpoints for getting information about your environment.</span></span> <span data-ttu-id="cc5db-156">Tento postup se nazývá hello *Kudu konzoly* nebo hello *řídicí panel SCM* pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cc5db-156">This is called hello *Kudu Console* or hello *SCM Dashboard* for your web app.</span></span>

<span data-ttu-id="cc5db-157">Tento řídicí panel můžete přejít pomocí odkazu přejdete toohello **https://&lt;název aplikace >.scm.azurewebsites.net/**.</span><span class="sxs-lookup"><span data-stu-id="cc5db-157">You can access this dashboard by going toohello link **https://&lt;Your app name>.scm.azurewebsites.net/**.</span></span>

<span data-ttu-id="cc5db-158">Jsou některé hello věcí, které Kudu poskytuje:</span><span class="sxs-lookup"><span data-stu-id="cc5db-158">Some of hello things that Kudu provides are:</span></span>

* <span data-ttu-id="cc5db-159">nastavení prostředí pro vaši aplikaci</span><span class="sxs-lookup"><span data-stu-id="cc5db-159">environment settings for your application</span></span>
* <span data-ttu-id="cc5db-160">datový proud protokolu</span><span class="sxs-lookup"><span data-stu-id="cc5db-160">log stream</span></span>
* <span data-ttu-id="cc5db-161">diagnostické výpis</span><span class="sxs-lookup"><span data-stu-id="cc5db-161">diagnostic dump</span></span>
* <span data-ttu-id="cc5db-162">ladění konzoly, ve kterém můžete spouštět rutiny prostředí Powershell a základních příkazů DOS.</span><span class="sxs-lookup"><span data-stu-id="cc5db-162">debug console in which you can run Powershell cmdlets and basic DOS commands.</span></span>

<span data-ttu-id="cc5db-163">Další užitečné funkce Kudu je, že v případě, že aplikace je vyvolání first chance výjimek, můžete použít Kudu a výpisů hello SysInternals nástroj Procdump toocreate paměti.</span><span class="sxs-lookup"><span data-stu-id="cc5db-163">Another useful feature of Kudu is that, in case your application is throwing first-chance exceptions, you can use Kudu and hello SysInternals tool Procdump toocreate memory dumps.</span></span> <span data-ttu-id="cc5db-164">Tyto výpisy paměti jsou snímky hello procesu a často může pomoci při odstraňování složitějších problémů s vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="cc5db-164">These memory dumps are snapshots of hello process and can often help you troubleshoot more complicated issues with your web app.</span></span>

<span data-ttu-id="cc5db-165">Další informace o funkcích, které jsou k dispozici v Kudu, najdete v části [online nástroje weby Azure, měli byste vědět o](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/).</span><span class="sxs-lookup"><span data-stu-id="cc5db-165">For more information on features available in Kudu, see [Azure Websites online tools you should know about](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/).</span></span>

<a name="mitigate" />

### <a name="3-mitigate-hello-issue"></a><span data-ttu-id="cc5db-166">3. Zmírnění hello problém</span><span class="sxs-lookup"><span data-stu-id="cc5db-166">3. Mitigate hello issue</span></span>
#### <a name="scale-hello-web-app"></a><span data-ttu-id="cc5db-167">Škálování hello webové aplikace</span><span class="sxs-lookup"><span data-stu-id="cc5db-167">Scale hello web app</span></span>
<span data-ttu-id="cc5db-168">Ve službě Azure App Service pro vyšší výkon a propustnost, můžete upravit hello škálování, ve kterém je spuštěná vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="cc5db-168">In Azure App Service, for increased performance and throughput,  you can adjust hello scale at which you are running your application.</span></span> <span data-ttu-id="cc5db-169">Škálování webovou aplikaci zahrnuje dvě souvisejících akcích: Změna vašeho plánu služby App Service tooa vyšší cenové úrovně a konfigurace určitá nastavení po Přepnuli jste toohello vyšší cenová úroveň.</span><span class="sxs-lookup"><span data-stu-id="cc5db-169">Scaling up a web app involves two related actions: changing your App Service plan tooa higher pricing tier, and configuring certain settings after you have switched toohello higher pricing tier.</span></span>

<span data-ttu-id="cc5db-170">Další informace o škálování najdete v tématu [škálování webové aplikace v Azure App Service](web-sites-scale.md).</span><span class="sxs-lookup"><span data-stu-id="cc5db-170">For more information on scaling, see [Scale a web app in Azure App Service](web-sites-scale.md).</span></span>

<span data-ttu-id="cc5db-171">Kromě toho můžete zvolit toorun aplikaci na více než jednu instanci.</span><span class="sxs-lookup"><span data-stu-id="cc5db-171">Additionally, you can choose toorun your application on more than one instance .</span></span> <span data-ttu-id="cc5db-172">Pouze to poskytuje další možnost zpracování, ale také vám dává některé množství odolnost proti chybám.</span><span class="sxs-lookup"><span data-stu-id="cc5db-172">This not only provides you with more processing capability, but also gives you some amount of fault tolerance.</span></span> <span data-ttu-id="cc5db-173">Pokud hello proces přestane fungovat na jednu instanci, hello ostatní instance stále bude obsluhovat požadavky.</span><span class="sxs-lookup"><span data-stu-id="cc5db-173">If hello process goes down on one instance, hello other instance will still continue serving requests.</span></span>

<span data-ttu-id="cc5db-174">Můžete nastavit hello toobe ruční nebo automatické škálování.</span><span class="sxs-lookup"><span data-stu-id="cc5db-174">You can set hello scaling toobe Manual or Automatic.</span></span>

#### <a name="use-autoheal"></a><span data-ttu-id="cc5db-175">Pomocí funkce AutoHeal</span><span class="sxs-lookup"><span data-stu-id="cc5db-175">Use AutoHeal</span></span>
<span data-ttu-id="cc5db-176">Funkce AutoHeal recykluje pracovní proces hello pro vaši aplikaci na základě nastavení, které zvolíte (jako jsou změny konfigurace, požadavky, omezení na základě paměti nebo hello čas potřeby tooexecute požadavek).</span><span class="sxs-lookup"><span data-stu-id="cc5db-176">AutoHeal recycles hello worker process for your app based on settings you choose (like configuration changes, requests, memory-based limits, or hello time needed tooexecute a request).</span></span> <span data-ttu-id="cc5db-177">Většinu času hello recyklaci hello proces je hello nejrychlejší způsob, jak toorecover po chybě.</span><span class="sxs-lookup"><span data-stu-id="cc5db-177">Most of hello time, recycle hello process is hello fastest way toorecover from a problem.</span></span> <span data-ttu-id="cc5db-178">I když můžete vždy restartovat hello webové aplikace z přímo v rámci hello portálu Azure, funkce AutoHeal bude provádět automaticky za vás.</span><span class="sxs-lookup"><span data-stu-id="cc5db-178">Though you can always restart hello web app from directly within hello Azure Portal, AutoHeal will do it automatically for you.</span></span> <span data-ttu-id="cc5db-179">Toodo stačí je přidat některé aktivační události v hello kořenovém souboru web.config pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cc5db-179">All you need toodo is add some triggers in hello root web.config for your web app.</span></span> <span data-ttu-id="cc5db-180">Všimněte si, že toto nastavení by fungovat v hello stejný způsobem, i když se vaše aplikace není jeden .net.</span><span class="sxs-lookup"><span data-stu-id="cc5db-180">Note that these settings would work in hello same way even if your application is not a .Net one.</span></span>

<span data-ttu-id="cc5db-181">Další informace najdete v tématu [automatické opravy weby Azure](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/).</span><span class="sxs-lookup"><span data-stu-id="cc5db-181">For more information, see [Auto-Healing Azure Web Sites](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/).</span></span>

#### <a name="restart-hello-web-app"></a><span data-ttu-id="cc5db-182">Restartujte hello webové aplikace</span><span class="sxs-lookup"><span data-stu-id="cc5db-182">Restart hello web app</span></span>
<span data-ttu-id="cc5db-183">Tento problém je často hello nejjednodušší způsob, jak toorecover z jednorázových problémů.</span><span class="sxs-lookup"><span data-stu-id="cc5db-183">This is often hello simplest way toorecover from one-time issues.</span></span> <span data-ttu-id="cc5db-184">Na hello [portálu Azure](https://portal.azure.com/), v okně vaší webové aplikace, máte hello možnosti toostop nebo restartujte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cc5db-184">On hello [Azure Portal](https://portal.azure.com/), on your web app’s blade, you have hello options toostop or restart your app.</span></span>

 ![Restartujte aplikace toosolve chyby protokolu HTTP 502 Chybná brána a 503 Služba nedostupná](./media/app-service-web-troubleshoot-HTTP-502-503/2-restart.png)

<span data-ttu-id="cc5db-186">Můžete také spravovat webové aplikace pomocí Azure Powershell.</span><span class="sxs-lookup"><span data-stu-id="cc5db-186">You can also manage your web app using Azure Powershell.</span></span> <span data-ttu-id="cc5db-187">Další informace najdete v tématu [Použití Azure PowerShellu s Azure Resource Managerem](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="cc5db-187">For more information, see [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md).</span></span>

