---
title: "Použití Log Analytics s aplikací s více tenanty využívající službu SQL Database | Dokumentace Microsoftu"
description: "Instalace a použití analýzy protokolů (OMS) s Azure SQL Database ukázková Wingtip SaaS aplikace"
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
ms.openlocfilehash: 26f6f519ecb3abf6343dc2776aa141dff99ced15
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="setup-and-use-log-analytics-oms-with-the-wingtip-saas-app"></a><span data-ttu-id="3bd01-104">Instalace a použití analýzy protokolů (OMS) s Wingtip SaaS aplikace</span><span class="sxs-lookup"><span data-stu-id="3bd01-104">Setup and use Log Analytics (OMS) with the Wingtip SaaS app</span></span>

<span data-ttu-id="3bd01-105">V tomto kurzu, nastavení a použití *analýzy protokolů ([OMS](https://www.microsoft.com/cloud-platform/operations-management-suite))* pro monitorování elastické fondy a databází.</span><span class="sxs-lookup"><span data-stu-id="3bd01-105">In this tutorial, you set up and use *Log Analytics([OMS](https://www.microsoft.com/cloud-platform/operations-management-suite))* for monitoring elastic pools and databases.</span></span> <span data-ttu-id="3bd01-106">Je nástavbou kurzu [Monitorování a správa výkonu](sql-database-saas-tutorial-performance-monitoring.md) a uvádí, jak se používá *Log Analytics* k posílení monitorování a upozorňování poskytovaného na portálu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="3bd01-106">It builds on the [Performance Monitoring and Management tutorial](sql-database-saas-tutorial-performance-monitoring.md), and shows how to use *Log Analytics* to augment the monitoring and alerting provided in the Azure portal.</span></span> <span data-ttu-id="3bd01-107">Služba Log Analytics je zvlášť vhodná pro monitorování a upozorňování v různém rozsahu, protože podporuje stovky fondů a stovky tisíc databází.</span><span class="sxs-lookup"><span data-stu-id="3bd01-107">Log Analytics is particularly suitable for monitoring and alerting at scale because it supports hundreds of pools and hundreds of thousands of databases.</span></span> <span data-ttu-id="3bd01-108">Poskytuje také jedno řešení pro monitorování, které může integrovat monitorování různých aplikací a služeb Azure napříč několika předplatnými Azure.</span><span class="sxs-lookup"><span data-stu-id="3bd01-108">It also provides a single monitoring solution, which can integrate monitoring of different applications and Azure services, across multiple Azure subscriptions.</span></span>

<span data-ttu-id="3bd01-109">Co se v tomto kurzu naučíte:</span><span class="sxs-lookup"><span data-stu-id="3bd01-109">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3bd01-110">Instalace a konfigurace Log Analytics (OMS)</span><span class="sxs-lookup"><span data-stu-id="3bd01-110">Install and configure Log Analytics (OMS)</span></span>
> * <span data-ttu-id="3bd01-111">Monitorování fondů a databází pomocí Log Analytics</span><span class="sxs-lookup"><span data-stu-id="3bd01-111">Use Log Analytics to monitor pools and databases</span></span>

<span data-ttu-id="3bd01-112">Předpokladem dokončení tohoto kurzu je splnění následujících požadavků:</span><span class="sxs-lookup"><span data-stu-id="3bd01-112">To complete this tutorial, make sure the following prerequisites are completed:</span></span>

* <span data-ttu-id="3bd01-113">Adresář Wingtip SaaS aplikace je nasazená.</span><span class="sxs-lookup"><span data-stu-id="3bd01-113">The Wingtip SaaS app is deployed.</span></span> <span data-ttu-id="3bd01-114">Nasazení za méně než pět minut najdete v tématu [nasazení a seznamte se s Wingtip SaaS aplikace](sql-database-saas-tutorial.md)</span><span class="sxs-lookup"><span data-stu-id="3bd01-114">To deploy in less than five minutes, see [Deploy and explore the Wingtip SaaS application](sql-database-saas-tutorial.md)</span></span>
* <span data-ttu-id="3bd01-115">Je nainstalované prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3bd01-115">Azure PowerShell is installed.</span></span> <span data-ttu-id="3bd01-116">Podrobnosti najdete v článku [Začínáme s prostředím Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).</span><span class="sxs-lookup"><span data-stu-id="3bd01-116">For details, see [Getting started with Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)</span></span>

<span data-ttu-id="3bd01-117">Diskusi ke scénářům a vzorcům SaaS a způsob, jakým ovlivňují požadavky na řešení pro monitorování, najdete v [kurzu Monitorování a správa výkonu](sql-database-saas-tutorial-performance-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="3bd01-117">See the [Performance Monitoring and Management tutorial](sql-database-saas-tutorial-performance-monitoring.md) for a discussion of the SaaS scenarios and patterns, and how they affect the requirements on a monitoring solution.</span></span>

## <a name="monitoring-and-managing-performance-with-log-analytics-oms"></a><span data-ttu-id="3bd01-118">Monitorování a správa výkonu pomocí Log Analytics (OMS)</span><span class="sxs-lookup"><span data-stu-id="3bd01-118">Monitoring and managing performance with Log Analytics (OMS)</span></span>

<span data-ttu-id="3bd01-119">Ve službě SQL Database je monitorování a upozorňování k dispozici v databázích a fondech.</span><span class="sxs-lookup"><span data-stu-id="3bd01-119">For SQL Database, monitoring and alerting is available on databases and pools.</span></span> <span data-ttu-id="3bd01-120">Toto integrované monitorování a upozorňování je specifické podle prostředků a je vhodné pro malý počet prostředků, ale méně vhodné pro monitorování velkých instalací nebo poskytnutí sjednoceného pohledu na různé prostředky a předplatná.</span><span class="sxs-lookup"><span data-stu-id="3bd01-120">This built-in monitoring and alerting is resource-specific and convenient for small numbers of resources, but is less well suited to monitoring large installations or for providing a unified view across different resources and subscriptions.</span></span>

<span data-ttu-id="3bd01-121">Log Analytics je možné používat u velkoobjemových scénářů.</span><span class="sxs-lookup"><span data-stu-id="3bd01-121">For high-volume scenarios Log Analytics can be used.</span></span> <span data-ttu-id="3bd01-122">To je samostatná služba Azure, která poskytuje analýzy přes vydávané diagnostické protokoly a telemetrii získanou v pracovním prostoru Log Analytics, které můžou shromažďovat telemetrii z mnoha služeb a dají se používat k dotazování a nastavování výstrah.</span><span class="sxs-lookup"><span data-stu-id="3bd01-122">This is a separate Azure service that provides analytics over emitted diagnostic logs and telemetry gathered in a log analytics workspace, which can collect telemetry from many services and be used to query and set alerts.</span></span> <span data-ttu-id="3bd01-123">Log Analytics poskytuje integrovaný jazyk dotazů a nástroje vizualizace dat umožňující analýzy a vizualizaci provozních dat.</span><span class="sxs-lookup"><span data-stu-id="3bd01-123">Log Analytics provides a built-in query language and data visualization tools allowing operational data analytics and visualization.</span></span> <span data-ttu-id="3bd01-124">Řešení SQL Analytics nabízí několik předdefinovaných elastických fondů a zobrazení a dotazů na monitorování a upozorňování databáze a umožňuje přidávat vlastní ad-hoc dotazy a podle potřeby je ukládat.</span><span class="sxs-lookup"><span data-stu-id="3bd01-124">The SQL Analytics solution provides several pre-defined elastic pool and database monitoring and alerting views and queries, and lets you add your own ad-hoc queries and save these as needed.</span></span> <span data-ttu-id="3bd01-125">OMS poskytuje také vlastní návrhář zobrazení.</span><span class="sxs-lookup"><span data-stu-id="3bd01-125">OMS also provides a custom view designer.</span></span>

<span data-ttu-id="3bd01-126">Pracovní prostory a analytická řešení Log Analytics se dají otevírat z portálu Azure Portal i v OMS.</span><span class="sxs-lookup"><span data-stu-id="3bd01-126">Log Analytics workspaces and analytics solutions can be opened both in the Azure portal and in OMS.</span></span> <span data-ttu-id="3bd01-127">Azure Portal je novější přístupový bod, ale v některých oblastech může být za portálem OMS.</span><span class="sxs-lookup"><span data-stu-id="3bd01-127">The Azure portal is the newer access point but may be behind the OMS portal in some areas.</span></span>

### <a name="start-the-load-generator-to-create-data-to-analyze"></a><span data-ttu-id="3bd01-128">Spuštění generátoru zatížení pro vytváření dat k analýze</span><span class="sxs-lookup"><span data-stu-id="3bd01-128">Start the load generator to create data to analyze</span></span>

1. <span data-ttu-id="3bd01-129">Spusťte skript **Demo-PerformanceMonitoringAndManagement.ps1** ve složce **PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="3bd01-129">Open **Demo-PerformanceMonitoringAndManagement.ps1** in the **PowerShell ISE**.</span></span> <span data-ttu-id="3bd01-130">Tento skript nechte spuštěný, protože je možné, že během tohoto kurzu budete spouštět několik scénářů generování zatížení.</span><span class="sxs-lookup"><span data-stu-id="3bd01-130">Keep this script open as you may want to run several of the load generation scenarios during this tutorial.</span></span>
1. <span data-ttu-id="3bd01-131">Pokud používáte méně než pět tenantů, zřiďte dávku tenantů pro zajištění zajímavějšího kontextu monitorování:</span><span class="sxs-lookup"><span data-stu-id="3bd01-131">If you have less than five tenants, provision a batch of tenants to provide a more interesting monitoring context:</span></span>
   1. <span data-ttu-id="3bd01-132">Nastavte **$DemoScenario = 1,** **Zřízení dávky tenantů**</span><span class="sxs-lookup"><span data-stu-id="3bd01-132">Set **$DemoScenario = 1,** **Provision a batch of tenants**</span></span>
   1. <span data-ttu-id="3bd01-133">Stisknutím klávesy **F5** spusťte skript.</span><span class="sxs-lookup"><span data-stu-id="3bd01-133">Press **F5** to run the script.</span></span>

1. <span data-ttu-id="3bd01-134">Nastavte **$DemoScenario** = 2, **Generování normální intenzity zatížení (asi 40 DTU)**.</span><span class="sxs-lookup"><span data-stu-id="3bd01-134">Set **$DemoScenario** = 2, **Generate normal intensity load (approx 40 DTU)**.</span></span>
1. <span data-ttu-id="3bd01-135">Stisknutím klávesy **F5** spusťte skript.</span><span class="sxs-lookup"><span data-stu-id="3bd01-135">Press **F5** to run the script.</span></span>

## <a name="get-the-wingtip-application-scripts"></a><span data-ttu-id="3bd01-136">Získání skriptů aplikace Wingtip</span><span class="sxs-lookup"><span data-stu-id="3bd01-136">Get the Wingtip application scripts</span></span>

<span data-ttu-id="3bd01-137">Skripty a zdrojový kód aplikace Wingtip Tickets jsou dostupné v úložišti GitHubu [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS).</span><span class="sxs-lookup"><span data-stu-id="3bd01-137">The Wingtip Tickets scripts and application source code are available in the [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github repo.</span></span> <span data-ttu-id="3bd01-138">Soubory se skripty jsou ve [složce Learning Modules](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules).</span><span class="sxs-lookup"><span data-stu-id="3bd01-138">Script files are located in the [Learning Modules folder](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules).</span></span> <span data-ttu-id="3bd01-139">Stáhněte si složku **Learning Modules** do svého místního počítače. Dejte pozor, abyste zachovali strukturu složky.</span><span class="sxs-lookup"><span data-stu-id="3bd01-139">Download the **Learning Modules** folder to your local computer, maintaining its folder structure.</span></span>

## <a name="installing-and-configuring-log-analytics-and-the-azure-sql-analytics-solution"></a><span data-ttu-id="3bd01-140">Instalace a konfigurace Log Analytics a řešení Azure SQL Analytics</span><span class="sxs-lookup"><span data-stu-id="3bd01-140">Installing and configuring Log Analytics and the Azure SQL Analytics solution</span></span>

<span data-ttu-id="3bd01-141">Log Analytics je samostatná služba, která vyžaduje konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="3bd01-141">Log Analytics is a separate service that needs to be configured.</span></span> <span data-ttu-id="3bd01-142">Log Analytics shromažďuje data a telemetrii protokolů a metriku v pracovním prostoru Log Analytics.</span><span class="sxs-lookup"><span data-stu-id="3bd01-142">Log Analytics collects log data and telemetry and metrics in a log analytics workspace.</span></span> <span data-ttu-id="3bd01-143">Pracovní prostor je prostředek stejně jako ostatní prostředky v Azure a je potřeba ho vytvořit.</span><span class="sxs-lookup"><span data-stu-id="3bd01-143">A workspace is a resource, just like other resources in Azure, and must be created.</span></span> <span data-ttu-id="3bd01-144">I když pracovní prostor nemusí být vytvořený ve stejné skupině prostředků jako aplikace, které monitoruje, dává to často největší smysl.</span><span class="sxs-lookup"><span data-stu-id="3bd01-144">While the workspace doesn’t need to be created in the same resource group as the application(s) it is monitoring, this often makes the most sense.</span></span> <span data-ttu-id="3bd01-145">V případě aplikace Wingtip SaaS to umožňuje snadno odstranit s aplikace jednoduše odstraněním skupiny prostředků v pracovním prostoru.</span><span class="sxs-lookup"><span data-stu-id="3bd01-145">In the case of the Wingtip SaaS app, this enables the workspace to be easily deleted with the application by simply deleting the resource group.</span></span>

1. <span data-ttu-id="3bd01-146">Otevřete ...\\Learning Modules\\Performance Monitoring and Management\\Log Analytics\\*Demo-LogAnalytics.ps1* v **Integrovaném skriptovacím prostředí (ISE) v prostředí PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="3bd01-146">Open ...\\Learning Modules\\Performance Monitoring and Management\\Log Analytics\\*Demo-LogAnalytics.ps1* in the **PowerShell ISE**.</span></span>
1. <span data-ttu-id="3bd01-147">Stisknutím klávesy **F5** spusťte skript.</span><span class="sxs-lookup"><span data-stu-id="3bd01-147">Press **F5** to run the script.</span></span>

<span data-ttu-id="3bd01-148">Teď by mělo být možné otevřít Log Analytics na portálu Azure Portal (nebo portálu OMS).</span><span class="sxs-lookup"><span data-stu-id="3bd01-148">At this point you should be able open Log Analytics in the Azure portal (or the OMS portal).</span></span> <span data-ttu-id="3bd01-149">Shromáždění telemetrie a její zviditelnění v pracovním prostoru Log Analytics bude trvat pár minut.</span><span class="sxs-lookup"><span data-stu-id="3bd01-149">It will take a few minutes for telemetry to be collected in the Log Analytics workspace and to become visible.</span></span> <span data-ttu-id="3bd01-150">Čím déle necháte systém shromažďovat data, tím zajímavější bude práce s nimi.</span><span class="sxs-lookup"><span data-stu-id="3bd01-150">The longer you leave the system gathering data the more interesting the experience will be.</span></span> <span data-ttu-id="3bd01-151">Teď si můžete dát chvilku pauzu – jen se ujistěte, že je generátor pořád spuštěný!</span><span class="sxs-lookup"><span data-stu-id="3bd01-151">Now's a good time to grab a beverage - just make sure the load generator is still running!</span></span>


## <a name="use-log-analytics-and-the-sql-analytics-solution-to-monitor-pools-and-databases"></a><span data-ttu-id="3bd01-152">Monitorování fondů a databází pomocí Log Analytics a řešení SQL Analytics</span><span class="sxs-lookup"><span data-stu-id="3bd01-152">Use Log Analytics and the SQL Analytics solution to monitor pools and databases</span></span>


<span data-ttu-id="3bd01-153">V tomto cvičení otevřete analýzy protokolů a podívejte se na telemetrie shromážděných pro databáze a fondy na portálu OMS.</span><span class="sxs-lookup"><span data-stu-id="3bd01-153">In this exercise, open Log Analytics and the OMS portal to look at the telemetry being gathered for the databases and pools.</span></span>

1. <span data-ttu-id="3bd01-154">Přejděte na portál [Azure Portal](https://portal.azure.com) a otevřete Log Analytics kliknutím na Další služby. Potom vyhledejte Log Analytics:</span><span class="sxs-lookup"><span data-stu-id="3bd01-154">Browse to the [Azure portal](https://portal.azure.com) and open Log Analytics by clicking More services, then search for Log Analytics:</span></span>

   ![otevření log analytics](media/sql-database-saas-tutorial-log-analytics/log-analytics-open.png)

1. <span data-ttu-id="3bd01-156">Vyberte pracovní prostor s názvem *wtploganalytics-&lt;USER&gt;*.</span><span class="sxs-lookup"><span data-stu-id="3bd01-156">Select the workspace named *wtploganalytics-&lt;USER&gt;*.</span></span>

1. <span data-ttu-id="3bd01-157">Vyberte **Přehled** k otevření řešení Log Analytics na portálu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="3bd01-157">Select **Overview** to open the Log Analytics solution in the Azure portal.</span></span>
   <span data-ttu-id="3bd01-158">![overview-link](media/sql-database-saas-tutorial-log-analytics/click-overview.png)</span><span class="sxs-lookup"><span data-stu-id="3bd01-158">![overview-link](media/sql-database-saas-tutorial-log-analytics/click-overview.png)</span></span>

    <span data-ttu-id="3bd01-159">**DŮLEŽITÉ**: Může to několik minut trvat, než začne být řešení aktivní.</span><span class="sxs-lookup"><span data-stu-id="3bd01-159">**IMPORTANT**: It may take a couple of minutes before the solution is active.</span></span> <span data-ttu-id="3bd01-160">Buďte prosím trpěliví.</span><span class="sxs-lookup"><span data-stu-id="3bd01-160">Be patient!</span></span>

1. <span data-ttu-id="3bd01-161">Kliknutím otevřete dlaždici Azure SQL Analytics.</span><span class="sxs-lookup"><span data-stu-id="3bd01-161">Click on the Azure SQL Analytics tile to open it.</span></span>

    ![overview](media/sql-database-saas-tutorial-log-analytics/overview.png)

    ![analytics](media/sql-database-saas-tutorial-log-analytics/analytics.png)

1. <span data-ttu-id="3bd01-164">Zobrazení v okně řešení se prochází po straně a má vlastní posuvník v dolní části (pokud je to potřeba, aktualizujte okno).</span><span class="sxs-lookup"><span data-stu-id="3bd01-164">The view in the solution blade scrolls sideways, with its own scroll bar at the bottom (refresh the blade if needed).</span></span>

1. <span data-ttu-id="3bd01-165">Prozkoumejte různá zobrazení, a to tak, že na ně budete klikat nebo že budete klikat na jednotlivé prostředky. Otevře se průzkumník pro procházení k podrobnostem, kde se můžete pomocí časového posuvníku vlevo nahoře nebo kliknutím na svislý pruh zaměřit na užší časový interval.</span><span class="sxs-lookup"><span data-stu-id="3bd01-165">Explore the various views by clicking on them or on individual resources to open a drill-down explorer, where you can use the time-slider in the top left or click on a vertical bar to focus in on a narrower time slice.</span></span> <span data-ttu-id="3bd01-166">V tomto zobrazení můžete vybírat jednotlivé databáze nebo fondy pro zaměření na konkrétní prostředky:</span><span class="sxs-lookup"><span data-stu-id="3bd01-166">With this view, you can select individual databases or pools to focus on specific resources:</span></span>

    ![graf](media/sql-database-saas-tutorial-log-analytics/chart.png)

1. <span data-ttu-id="3bd01-168">Pokud budete zpět v okně řešení a přejdete zcela doprava, najdete některé uložené dotazy, které můžete kliknutím otevřít a prozkoumat.</span><span class="sxs-lookup"><span data-stu-id="3bd01-168">Back on the solution blade, if you scroll to the far right you will see some saved queries that you can click on to open and explore.</span></span> <span data-ttu-id="3bd01-169">Můžete je zkoušet měnit a můžete si uložit některé zajímavé dotazy, které vytvoříte, a později je znovu otevřít a používat s jinými prostředky.</span><span class="sxs-lookup"><span data-stu-id="3bd01-169">You can experiment with modifying these, and save any interesting queries you produce, which you can then re-open and use with other resources.</span></span>

1. <span data-ttu-id="3bd01-170">Až se vrátíte do okna pracovního prostoru Log Analytics, vyberte Portál OMS a otevřete řešení tady.</span><span class="sxs-lookup"><span data-stu-id="3bd01-170">Back on the Log Analytics workspace blade, select OMS Portal to open the solution there.</span></span>

    ![oms](media/sql-database-saas-tutorial-log-analytics/oms.png)

1. <span data-ttu-id="3bd01-172">Na portálu OMS můžete konfigurovat výstrahy.</span><span class="sxs-lookup"><span data-stu-id="3bd01-172">In the OMS portal, you can configure alerts.</span></span> <span data-ttu-id="3bd01-173">Klikněte na část výstrahy v zobrazení DTU databáze.</span><span class="sxs-lookup"><span data-stu-id="3bd01-173">Click on the alert portion of the database DTU view.</span></span>

1. <span data-ttu-id="3bd01-174">V zobrazení prohledávání protokolu uvidíte sloupcový graf představované metriky.</span><span class="sxs-lookup"><span data-stu-id="3bd01-174">In the Log Search view that appears you will see a bar graph of the metrics being represented.</span></span>

    ![prohledávání protokolů](media/sql-database-saas-tutorial-log-analytics/log-search.png)

1. <span data-ttu-id="3bd01-176">Pokud kliknete na výstrahu na panelu nástrojů, zobrazí se konfigurace výstrahy, kterou můžete změnit.</span><span class="sxs-lookup"><span data-stu-id="3bd01-176">If you click on Alert in the toolbar you will be able to see the alert configuration and can change it.</span></span>

    ![přidání pravidla výstrahy](media/sql-database-saas-tutorial-log-analytics/add-alert.png)

<span data-ttu-id="3bd01-178">Monitorování a upozorňování v Log Analytics a OMS je založené na dotazech na data v pracovním prostoru, na rozdíl od výstrah v oknech jednotlivých prostředků, které jsou specifické podle prostředků.</span><span class="sxs-lookup"><span data-stu-id="3bd01-178">The monitoring and alerting in Log Analytics and OMS is based on queries over the data in the workspace, unlike the alerting on each resource blade, which is resource-specific.</span></span> <span data-ttu-id="3bd01-179">Proto můžete definovat výstrahu, která dohlíží na všechny databáze místo definování samostatné výstrahy pro každou databázi.</span><span class="sxs-lookup"><span data-stu-id="3bd01-179">Thus, you can define an alert that looks over all databases, say, rather than defining one per database.</span></span> <span data-ttu-id="3bd01-180">Nebo můžete zapsat výstrahu, která používá složený dotaz přes více typů prostředků.</span><span class="sxs-lookup"><span data-stu-id="3bd01-180">Or write an alert that uses a composite query over multiple resource types.</span></span> <span data-ttu-id="3bd01-181">Dotazy jsou omezené jenom daty dostupnými v pracovním prostoru.</span><span class="sxs-lookup"><span data-stu-id="3bd01-181">Queries are only limited by the data available in the workspace.</span></span>

<span data-ttu-id="3bd01-182">Log Analytics pro SQL Database se účtuje podle objemu dat v pracovním prostoru.</span><span class="sxs-lookup"><span data-stu-id="3bd01-182">Log Analytics for SQL Database is charged for based on the data volume in the workspace.</span></span> <span data-ttu-id="3bd01-183">V tomto kurzu jste vytvořili bezplatný pracovní prostor omezený na 500 MB na den.</span><span class="sxs-lookup"><span data-stu-id="3bd01-183">In this tutorial, you created a Free workspace, which is limited to 500MB per day.</span></span> <span data-ttu-id="3bd01-184">Po dosažení tohoto limitu se data přestanou přidávat do pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="3bd01-184">Once that limit is reached data is no longer added to the workspace.</span></span>


## <a name="next-steps"></a><span data-ttu-id="3bd01-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3bd01-185">Next steps</span></span>

<span data-ttu-id="3bd01-186">V tomto kurzu jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="3bd01-186">In this tutorial you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3bd01-187">Instalace a konfigurace Log Analytics (OMS)</span><span class="sxs-lookup"><span data-stu-id="3bd01-187">Install and configure Log Analytics (OMS)</span></span>
> * <span data-ttu-id="3bd01-188">Monitorování fondů a databází pomocí Log Analytics</span><span class="sxs-lookup"><span data-stu-id="3bd01-188">Use Log Analytics to monitor pools and databases</span></span>

[<span data-ttu-id="3bd01-189">Kurz Analýza tenanta</span><span class="sxs-lookup"><span data-stu-id="3bd01-189">Tenant analytics tutorial</span></span>](sql-database-saas-tutorial-tenant-analytics.md)

## <a name="additional-resources"></a><span data-ttu-id="3bd01-190">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="3bd01-190">Additional resources</span></span>

* [<span data-ttu-id="3bd01-191">Další kurzy, které po počátečním nasazení aplikace Wingtip SaaS sestavení</span><span class="sxs-lookup"><span data-stu-id="3bd01-191">Additional tutorials that build upon the initial Wingtip SaaS application deployment</span></span>](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [<span data-ttu-id="3bd01-192">Azure Log Analytics</span><span class="sxs-lookup"><span data-stu-id="3bd01-192">Azure Log Analytics</span></span>](../log-analytics/log-analytics-azure-sql.md)
* [<span data-ttu-id="3bd01-193">OMS</span><span class="sxs-lookup"><span data-stu-id="3bd01-193">OMS</span></span>](https://blogs.technet.microsoft.com/msoms/2017/02/21/azure-sql-analytics-solution-public-preview/)
