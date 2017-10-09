---
title: "aaaUse analýzy protokolů a aplikaci víceklientské databáze SQL | Microsoft Docs"
description: "Instalace a použití analýzy protokolů (OMS) s hello Azure SQL Database ukázkové Wingtip SaaS aplikace"
keywords: kurz k sql database
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: billgib; sstein
ms.openlocfilehash: fa9085ce3462939e66853faa2a3cd71e0f6c2581
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="setup-and-use-log-analytics-oms-with-hello-wingtip-saas-app"></a><span data-ttu-id="4980c-104">Instalace a použití analýzy protokolů (OMS) s hello Wingtip SaaS aplikace</span><span class="sxs-lookup"><span data-stu-id="4980c-104">Setup and use Log Analytics (OMS) with hello Wingtip SaaS app</span></span>

<span data-ttu-id="4980c-105">V tomto kurzu, nastavení a použití *analýzy protokolů ([OMS](https://www.microsoft.com/cloud-platform/operations-management-suite))* pro monitorování elastické fondy a databází.</span><span class="sxs-lookup"><span data-stu-id="4980c-105">In this tutorial, you set up and use *Log Analytics([OMS](https://www.microsoft.com/cloud-platform/operations-management-suite))* for monitoring elastic pools and databases.</span></span> <span data-ttu-id="4980c-106">Vychází hello [sledování výkonu a správy kurzu](sql-database-saas-tutorial-performance-monitoring.md)a ukazuje, jak toouse *analýzy protokolů* tooaugment hello monitorování a výstrah, které jsou uvedeny v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4980c-106">It builds on hello [Performance Monitoring and Management tutorial](sql-database-saas-tutorial-performance-monitoring.md), and shows how toouse *Log Analytics* tooaugment hello monitoring and alerting provided in hello Azure portal.</span></span> <span data-ttu-id="4980c-107">Služba Log Analytics je zvlášť vhodná pro monitorování a upozorňování v různém rozsahu, protože podporuje stovky fondů a stovky tisíc databází.</span><span class="sxs-lookup"><span data-stu-id="4980c-107">Log Analytics is particularly suitable for monitoring and alerting at scale because it supports hundreds of pools and hundreds of thousands of databases.</span></span> <span data-ttu-id="4980c-108">Poskytuje také jedno řešení pro monitorování, které může integrovat monitorování různých aplikací a služeb Azure napříč několika předplatnými Azure.</span><span class="sxs-lookup"><span data-stu-id="4980c-108">It also provides a single monitoring solution, which can integrate monitoring of different applications and Azure services, across multiple Azure subscriptions.</span></span>

<span data-ttu-id="4980c-109">Co se v tomto kurzu naučíte:</span><span class="sxs-lookup"><span data-stu-id="4980c-109">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4980c-110">Instalace a konfigurace Log Analytics (OMS)</span><span class="sxs-lookup"><span data-stu-id="4980c-110">Install and configure Log Analytics (OMS)</span></span>
> * <span data-ttu-id="4980c-111">Pomocí analýzy protokolů toomonitor fondy a databáze</span><span class="sxs-lookup"><span data-stu-id="4980c-111">Use Log Analytics toomonitor pools and databases</span></span>

<span data-ttu-id="4980c-112">toocomplete dokončení tohoto kurzu, ujistěte se, hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="4980c-112">toocomplete this tutorial, make sure hello following prerequisites are completed:</span></span>

* <span data-ttu-id="4980c-113">Hello Wingtip SaaS aplikace je nasazená.</span><span class="sxs-lookup"><span data-stu-id="4980c-113">hello Wingtip SaaS app is deployed.</span></span> <span data-ttu-id="4980c-114">toodeploy za méně než pět minut, najdete v části [nasazení a seznamte se s hello Wingtip SaaS aplikace](sql-database-saas-tutorial.md)</span><span class="sxs-lookup"><span data-stu-id="4980c-114">toodeploy in less than five minutes, see [Deploy and explore hello Wingtip SaaS application](sql-database-saas-tutorial.md)</span></span>
* <span data-ttu-id="4980c-115">Je nainstalované prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4980c-115">Azure PowerShell is installed.</span></span> <span data-ttu-id="4980c-116">Podrobnosti najdete v článku [Začínáme s prostředím Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).</span><span class="sxs-lookup"><span data-stu-id="4980c-116">For details, see [Getting started with Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)</span></span>

<span data-ttu-id="4980c-117">V tématu hello [sledování výkonu a správy kurzu](sql-database-saas-tutorial-performance-monitoring.md) diskuzi o hello SaaS scénáře a vzory a jejich vlivu hello požadavky na řešení monitorování.</span><span class="sxs-lookup"><span data-stu-id="4980c-117">See hello [Performance Monitoring and Management tutorial](sql-database-saas-tutorial-performance-monitoring.md) for a discussion of hello SaaS scenarios and patterns, and how they affect hello requirements on a monitoring solution.</span></span>

## <a name="monitoring-and-managing-performance-with-log-analytics-oms"></a><span data-ttu-id="4980c-118">Monitorování a správa výkonu pomocí Log Analytics (OMS)</span><span class="sxs-lookup"><span data-stu-id="4980c-118">Monitoring and managing performance with Log Analytics (OMS)</span></span>

<span data-ttu-id="4980c-119">Ve službě SQL Database je monitorování a upozorňování k dispozici v databázích a fondech.</span><span class="sxs-lookup"><span data-stu-id="4980c-119">For SQL Database, monitoring and alerting is available on databases and pools.</span></span> <span data-ttu-id="4980c-120">Toto integrované monitorování a výstrah je konkrétní prostředky a vhodné pro malý počet prostředků, ale je méně rozsáhlé instalace skvěle hodí toomonitoring nebo pro zajištění jednotný pohled napříč různými prostředky a odběry.</span><span class="sxs-lookup"><span data-stu-id="4980c-120">This built-in monitoring and alerting is resource-specific and convenient for small numbers of resources, but is less well suited toomonitoring large installations or for providing a unified view across different resources and subscriptions.</span></span>

<span data-ttu-id="4980c-121">Log Analytics je možné používat u velkoobjemových scénářů.</span><span class="sxs-lookup"><span data-stu-id="4980c-121">For high-volume scenarios Log Analytics can be used.</span></span> <span data-ttu-id="4980c-122">Toto je samostatný služba Azure, která nabízí v porovnání s emitovaného diagnostické protokoly analýzy a telemetrie získané v pracovním prostoru analýzy protokolů, které může shromažďovat telemetrická data z mnoha služby a být použité tooquery a nastavit výstrahy.</span><span class="sxs-lookup"><span data-stu-id="4980c-122">This is a separate Azure service that provides analytics over emitted diagnostic logs and telemetry gathered in a log analytics workspace, which can collect telemetry from many services and be used tooquery and set alerts.</span></span> <span data-ttu-id="4980c-123">Log Analytics poskytuje integrovaný jazyk dotazů a nástroje vizualizace dat umožňující analýzy a vizualizaci provozních dat.</span><span class="sxs-lookup"><span data-stu-id="4980c-123">Log Analytics provides a built-in query language and data visualization tools allowing operational data analytics and visualization.</span></span> <span data-ttu-id="4980c-124">Hello řešení analýzy SQL poskytuje několik předem definovaných elastický fond a databáze, monitorování a výstrah, zobrazení a dotazy a umožňuje přidávat své vlastní dotazy ad hoc a uložit tyto podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="4980c-124">hello SQL Analytics solution provides several pre-defined elastic pool and database monitoring and alerting views and queries, and lets you add your own ad-hoc queries and save these as needed.</span></span> <span data-ttu-id="4980c-125">OMS poskytuje také vlastní návrhář zobrazení.</span><span class="sxs-lookup"><span data-stu-id="4980c-125">OMS also provides a custom view designer.</span></span>

<span data-ttu-id="4980c-126">Pracovní prostory a analýzy analytická řešení protokolu lze otevřít v hello portál Azure a v OMS.</span><span class="sxs-lookup"><span data-stu-id="4980c-126">Log Analytics workspaces and analytics solutions can be opened both in hello Azure portal and in OMS.</span></span> <span data-ttu-id="4980c-127">Hello portál Azure je hello novější přístupový bod, ale mohou být za portálu OMS hello v určité oblasti.</span><span class="sxs-lookup"><span data-stu-id="4980c-127">hello Azure portal is hello newer access point but may be behind hello OMS portal in some areas.</span></span>

### <a name="start-hello-load-generator-toocreate-data-tooanalyze"></a><span data-ttu-id="4980c-128">Spustit hello zatížení generátor toocreate data tooanalyze</span><span class="sxs-lookup"><span data-stu-id="4980c-128">Start hello load generator toocreate data tooanalyze</span></span>

1. <span data-ttu-id="4980c-129">Otevřete **ukázku PerformanceMonitoringAndManagement.ps1** v hello **prostředí PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="4980c-129">Open **Demo-PerformanceMonitoringAndManagement.ps1** in hello **PowerShell ISE**.</span></span> <span data-ttu-id="4980c-130">Uchovávejte tento skript otevřete tak, jak chcete toorun několik scénářů generování zatížení hello během tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="4980c-130">Keep this script open as you may want toorun several of hello load generation scenarios during this tutorial.</span></span>
1. <span data-ttu-id="4980c-131">Pokud máte méně než pět klienty, zřídit dávky klienty tooprovide zajímavějšího monitorování kontextu:</span><span class="sxs-lookup"><span data-stu-id="4980c-131">If you have less than five tenants, provision a batch of tenants tooprovide a more interesting monitoring context:</span></span>
   1. <span data-ttu-id="4980c-132">Nastavte **$DemoScenario = 1,** **Zřízení dávky tenantů**</span><span class="sxs-lookup"><span data-stu-id="4980c-132">Set **$DemoScenario = 1,** **Provision a batch of tenants**</span></span>
   1. <span data-ttu-id="4980c-133">Stiskněte klávesu **F5** toorun hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="4980c-133">Press **F5** toorun hello script.</span></span>

1. <span data-ttu-id="4980c-134">Nastavte **$DemoScenario** = 2, **Generování normální intenzity zatížení (asi 40 DTU)**.</span><span class="sxs-lookup"><span data-stu-id="4980c-134">Set **$DemoScenario** = 2, **Generate normal intensity load (approx 40 DTU)**.</span></span>
1. <span data-ttu-id="4980c-135">Stiskněte klávesu **F5** toorun hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="4980c-135">Press **F5** toorun hello script.</span></span>

## <a name="get-hello-wingtip-application-scripts"></a><span data-ttu-id="4980c-136">Získat hello Wingtip aplikační skripty</span><span class="sxs-lookup"><span data-stu-id="4980c-136">Get hello Wingtip application scripts</span></span>

<span data-ttu-id="4980c-137">Hello Wingtip lístky skripty a zdrojový kód aplikace jsou k dispozici v hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) úložiště github.</span><span class="sxs-lookup"><span data-stu-id="4980c-137">hello Wingtip Tickets scripts and application source code are available in hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github repo.</span></span> <span data-ttu-id="4980c-138">Skript soubory jsou umístěny v hello [Learning moduly složky](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules).</span><span class="sxs-lookup"><span data-stu-id="4980c-138">Script files are located in hello [Learning Modules folder](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules).</span></span> <span data-ttu-id="4980c-139">Stáhnout hello **Learning moduly** místní počítač tooyour složky, údržby jeho strukturu složek.</span><span class="sxs-lookup"><span data-stu-id="4980c-139">Download hello **Learning Modules** folder tooyour local computer, maintaining its folder structure.</span></span>

## <a name="installing-and-configuring-log-analytics-and-hello-azure-sql-analytics-solution"></a><span data-ttu-id="4980c-140">Instalace a konfigurace analýzy protokolů a hello řešení analýzy SQL Azure</span><span class="sxs-lookup"><span data-stu-id="4980c-140">Installing and configuring Log Analytics and hello Azure SQL Analytics solution</span></span>

<span data-ttu-id="4980c-141">Analýzy protokolů je samostatné služby, který vyžaduje toobe nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="4980c-141">Log Analytics is a separate service that needs toobe configured.</span></span> <span data-ttu-id="4980c-142">Log Analytics shromažďuje data a telemetrii protokolů a metriku v pracovním prostoru Log Analytics.</span><span class="sxs-lookup"><span data-stu-id="4980c-142">Log Analytics collects log data and telemetry and metrics in a log analytics workspace.</span></span> <span data-ttu-id="4980c-143">Pracovní prostor je prostředek stejně jako ostatní prostředky v Azure a je potřeba ho vytvořit.</span><span class="sxs-lookup"><span data-stu-id="4980c-143">A workspace is a resource, just like other resources in Azure, and must be created.</span></span> <span data-ttu-id="4980c-144">Při hello prostoru nepotřebuje toobe vytvořené v hello stejné skupině prostředků jako aplikace hello je monitorování, tato možnost může hello nejvhodnější.</span><span class="sxs-lookup"><span data-stu-id="4980c-144">While hello workspace doesn’t need toobe created in hello same resource group as hello application(s) it is monitoring, this often makes hello most sense.</span></span> <span data-ttu-id="4980c-145">V případě hello hello Wingtip SaaS aplikace to umožňuje toobe prostoru hello snadno odstranit aplikaci hello jednoduše odstraněním skupiny prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="4980c-145">In hello case of hello Wingtip SaaS app, this enables hello workspace toobe easily deleted with hello application by simply deleting hello resource group.</span></span>

1. <span data-ttu-id="4980c-146">Otevřete... \\Learning moduly\\sledování výkonu a správy\\analýzy protokolů\\*ukázku LogAnalytics.ps1* v hello **prostředí PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="4980c-146">Open ...\\Learning Modules\\Performance Monitoring and Management\\Log Analytics\\*Demo-LogAnalytics.ps1* in hello **PowerShell ISE**.</span></span>
1. <span data-ttu-id="4980c-147">Stiskněte klávesu **F5** toorun hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="4980c-147">Press **F5** toorun hello script.</span></span>

<span data-ttu-id="4980c-148">V tomto okamžiku byste měli mít možnost Otevřít analýzy protokolů hello portálu Azure (nebo portálu OMS hello).</span><span class="sxs-lookup"><span data-stu-id="4980c-148">At this point you should be able open Log Analytics in hello Azure portal (or hello OMS portal).</span></span> <span data-ttu-id="4980c-149">To bude trvat několik minut, než telemetrie toobe shromážděny v hello pracovní prostor analýzy protokolů a toobecome viditelné.</span><span class="sxs-lookup"><span data-stu-id="4980c-149">It will take a few minutes for telemetry toobe collected in hello Log Analytics workspace and toobecome visible.</span></span> <span data-ttu-id="4980c-150">Hello delší že necháte hello systému shromažďování, že data hello zajímavějšího hello prostředí bude.</span><span class="sxs-lookup"><span data-stu-id="4980c-150">hello longer you leave hello system gathering data hello more interesting hello experience will be.</span></span> <span data-ttu-id="4980c-151">Teď je vhodná doba toograb nápoj - jenom Ujistěte se, generátor zatížení hello je stále spuštěná!</span><span class="sxs-lookup"><span data-stu-id="4980c-151">Now's a good time toograb a beverage - just make sure hello load generator is still running!</span></span>


## <a name="use-log-analytics-and-hello-sql-analytics-solution-toomonitor-pools-and-databases"></a><span data-ttu-id="4980c-152">Pomocí analýzy protokolů a hello fondy toomonitor řešení SQL Analytics a databáze</span><span class="sxs-lookup"><span data-stu-id="4980c-152">Use Log Analytics and hello SQL Analytics solution toomonitor pools and databases</span></span>


<span data-ttu-id="4980c-153">V tomto cvičení otevřete analýzy protokolů a hello toolook portálu OMS na telemetrie hello shromážděných pro hello databáze a fondy.</span><span class="sxs-lookup"><span data-stu-id="4980c-153">In this exercise, open Log Analytics and hello OMS portal toolook at hello telemetry being gathered for hello databases and pools.</span></span>

1. <span data-ttu-id="4980c-154">Procházet toohello [portál Azure](https://portal.azure.com) a otevřete analýzy protokolů tak, že kliknete na další služby a potom najděte analýzy protokolů:</span><span class="sxs-lookup"><span data-stu-id="4980c-154">Browse toohello [Azure portal](https://portal.azure.com) and open Log Analytics by clicking More services, then search for Log Analytics:</span></span>

   ![otevření log analytics](media/sql-database-saas-tutorial-log-analytics/log-analytics-open.png)

1. <span data-ttu-id="4980c-156">Vyberte pracovní prostor hello s názvem *wtploganalytics -&lt;uživatele&gt;*.</span><span class="sxs-lookup"><span data-stu-id="4980c-156">Select hello workspace named *wtploganalytics-&lt;USER&gt;*.</span></span>

1. <span data-ttu-id="4980c-157">Vyberte **přehled** tooopen hello analýzy protokolů řešení v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4980c-157">Select **Overview** tooopen hello Log Analytics solution in hello Azure portal.</span></span>
   <span data-ttu-id="4980c-158">![overview-link](media/sql-database-saas-tutorial-log-analytics/click-overview.png)</span><span class="sxs-lookup"><span data-stu-id="4980c-158">![overview-link](media/sql-database-saas-tutorial-log-analytics/click-overview.png)</span></span>

    <span data-ttu-id="4980c-159">**Důležité**: může trvat několik minut, než je aktivní hello řešení.</span><span class="sxs-lookup"><span data-stu-id="4980c-159">**IMPORTANT**: It may take a couple of minutes before hello solution is active.</span></span> <span data-ttu-id="4980c-160">Buďte prosím trpěliví.</span><span class="sxs-lookup"><span data-stu-id="4980c-160">Be patient!</span></span>

1. <span data-ttu-id="4980c-161">Klikněte na hello Azure SQL Analytics dlaždice tooopen ho.</span><span class="sxs-lookup"><span data-stu-id="4980c-161">Click on hello Azure SQL Analytics tile tooopen it.</span></span>

    ![overview](media/sql-database-saas-tutorial-log-analytics/overview.png)

    ![analytics](media/sql-database-saas-tutorial-log-analytics/analytics.png)

1. <span data-ttu-id="4980c-164">Hello zobrazení v hello řešení okno posouvá do stran, s vlastní posuvníku v dolní části hello (aktualizace hello okně potřeby).</span><span class="sxs-lookup"><span data-stu-id="4980c-164">hello view in hello solution blade scrolls sideways, with its own scroll bar at hello bottom (refresh hello blade if needed).</span></span>

1. <span data-ttu-id="4980c-165">Prozkoumejte svislé hello různých zobrazení kliknutím na nich nebo na jednotlivé prostředky tooopen procházení explorer, kde můžete použít hello čas posuvníku v hello top doleva nebo klikněte na panel toofocus v na užší časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="4980c-165">Explore hello various views by clicking on them or on individual resources tooopen a drill-down explorer, where you can use hello time-slider in hello top left or click on a vertical bar toofocus in on a narrower time slice.</span></span> <span data-ttu-id="4980c-166">V tomto zobrazení si můžete vybrat jednotlivé databáze nebo fondy toofocus na konkrétní prostředky:</span><span class="sxs-lookup"><span data-stu-id="4980c-166">With this view, you can select individual databases or pools toofocus on specific resources:</span></span>

    ![graf](media/sql-database-saas-tutorial-log-analytics/chart.png)

1. <span data-ttu-id="4980c-168">Zpět v okně řešení hello Pokud se posunete toohello nejvíce vpravo uvidíte některé uložené dotazy, můžete kliknutím na tooopen a prozkoumat.</span><span class="sxs-lookup"><span data-stu-id="4980c-168">Back on hello solution blade, if you scroll toohello far right you will see some saved queries that you can click on tooopen and explore.</span></span> <span data-ttu-id="4980c-169">Můžete je zkoušet měnit a můžete si uložit některé zajímavé dotazy, které vytvoříte, a později je znovu otevřít a používat s jinými prostředky.</span><span class="sxs-lookup"><span data-stu-id="4980c-169">You can experiment with modifying these, and save any interesting queries you produce, which you can then re-open and use with other resources.</span></span>

1. <span data-ttu-id="4980c-170">Zpět v okně pracovní prostor analýzy protokolů hello vyberte portálu OMS tooopen hello řešení existuje.</span><span class="sxs-lookup"><span data-stu-id="4980c-170">Back on hello Log Analytics workspace blade, select OMS Portal tooopen hello solution there.</span></span>

    ![oms](media/sql-database-saas-tutorial-log-analytics/oms.png)

1. <span data-ttu-id="4980c-172">Na portálu OMS hello můžete nakonfigurovat výstrahy.</span><span class="sxs-lookup"><span data-stu-id="4980c-172">In hello OMS portal, you can configure alerts.</span></span> <span data-ttu-id="4980c-173">Klikněte na hello výstrahy část hello databáze DTU zobrazení.</span><span class="sxs-lookup"><span data-stu-id="4980c-173">Click on hello alert portion of hello database DTU view.</span></span>

1. <span data-ttu-id="4980c-174">V hello protokol hledání se zobrazí zobrazení, které se vám zobrazí pruhový graf reprezentované metrik hello.</span><span class="sxs-lookup"><span data-stu-id="4980c-174">In hello Log Search view that appears you will see a bar graph of hello metrics being represented.</span></span>

    ![prohledávání protokolů](media/sql-database-saas-tutorial-log-analytics/log-search.png)

1. <span data-ttu-id="4980c-176">Pokud kliknete na výstrahu v panelu nástrojů hello bude možné toosee hello výstrah Konfigurace a můžete ho změnit.</span><span class="sxs-lookup"><span data-stu-id="4980c-176">If you click on Alert in hello toolbar you will be able toosee hello alert configuration and can change it.</span></span>

    ![přidání pravidla výstrahy](media/sql-database-saas-tutorial-log-analytics/add-alert.png)

<span data-ttu-id="4980c-178">Hello monitorování a generování výstrah v analýzy protokolů a OMS je založená na dotazech nad daty hello v hello prostoru, na rozdíl od hello výstrahy v okně každý prostředek, který je konkrétní prostředky.</span><span class="sxs-lookup"><span data-stu-id="4980c-178">hello monitoring and alerting in Log Analytics and OMS is based on queries over hello data in hello workspace, unlike hello alerting on each resource blade, which is resource-specific.</span></span> <span data-ttu-id="4980c-179">Proto můžete definovat výstrahu, která dohlíží na všechny databáze místo definování samostatné výstrahy pro každou databázi.</span><span class="sxs-lookup"><span data-stu-id="4980c-179">Thus, you can define an alert that looks over all databases, say, rather than defining one per database.</span></span> <span data-ttu-id="4980c-180">Nebo můžete zapsat výstrahu, která používá složený dotaz přes více typů prostředků.</span><span class="sxs-lookup"><span data-stu-id="4980c-180">Or write an alert that uses a composite query over multiple resource types.</span></span> <span data-ttu-id="4980c-181">Dotazy jsou omezena pouze hello data k dispozici v pracovním prostoru hello.</span><span class="sxs-lookup"><span data-stu-id="4980c-181">Queries are only limited by hello data available in hello workspace.</span></span>

<span data-ttu-id="4980c-182">Analýzy protokolů pro databáze SQL je účtovat na základě objemu dat hello v prostoru hello.</span><span class="sxs-lookup"><span data-stu-id="4980c-182">Log Analytics for SQL Database is charged for based on hello data volume in hello workspace.</span></span> <span data-ttu-id="4980c-183">V tomto kurzu jste vytvořili volného prostoru, což je omezená too500MB za den.</span><span class="sxs-lookup"><span data-stu-id="4980c-183">In this tutorial, you created a Free workspace, which is limited too500MB per day.</span></span> <span data-ttu-id="4980c-184">Po dosažení tohoto limitu data je již přidána toohello prostoru.</span><span class="sxs-lookup"><span data-stu-id="4980c-184">Once that limit is reached data is no longer added toohello workspace.</span></span>


## <a name="next-steps"></a><span data-ttu-id="4980c-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4980c-185">Next steps</span></span>

<span data-ttu-id="4980c-186">V tomto kurzu jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="4980c-186">In this tutorial you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4980c-187">Instalace a konfigurace Log Analytics (OMS)</span><span class="sxs-lookup"><span data-stu-id="4980c-187">Install and configure Log Analytics (OMS)</span></span>
> * <span data-ttu-id="4980c-188">Pomocí analýzy protokolů toomonitor fondy a databáze</span><span class="sxs-lookup"><span data-stu-id="4980c-188">Use Log Analytics toomonitor pools and databases</span></span>

[<span data-ttu-id="4980c-189">Kurz Analýza tenanta</span><span class="sxs-lookup"><span data-stu-id="4980c-189">Tenant analytics tutorial</span></span>](sql-database-saas-tutorial-tenant-analytics.md)

## <a name="additional-resources"></a><span data-ttu-id="4980c-190">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="4980c-190">Additional resources</span></span>

* [<span data-ttu-id="4980c-191">Další kurzy, které stavějí hello počáteční nasazení Wingtip SaaS aplikace</span><span class="sxs-lookup"><span data-stu-id="4980c-191">Additional tutorials that build upon hello initial Wingtip SaaS application deployment</span></span>](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [<span data-ttu-id="4980c-192">Azure Log Analytics</span><span class="sxs-lookup"><span data-stu-id="4980c-192">Azure Log Analytics</span></span>](../log-analytics/log-analytics-azure-sql.md)
* [<span data-ttu-id="4980c-193">OMS</span><span class="sxs-lookup"><span data-stu-id="4980c-193">OMS</span></span>](https://blogs.technet.microsoft.com/msoms/2017/02/21/azure-sql-analytics-solution-public-preview/)
