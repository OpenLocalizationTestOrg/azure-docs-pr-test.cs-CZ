---
title: "Řešení Azure SQL analýzy v Log Analytics | Microsoft Docs"
description: "Řešení analýzy SQL Azure pomáhá spravovat vaše databáze Azure SQL."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: b2712749-1ded-40c4-b211-abc51cc65171
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: banders
ms.openlocfilehash: cab45cc6dd621eb4a95ef5f1842ec38c25e980b6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="monitor-azure-sql-database-using-azure-sql-analytics-preview-in-log-analytics"></a><span data-ttu-id="edf95-103">Monitorování databáze Azure SQL pomocí analýzy SQL Azure (Preview) v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="edf95-103">Monitor Azure SQL Database using Azure SQL Analytics (Preview) in Log Analytics</span></span>

![Azure SQL Analytics symbol](./media/log-analytics-azure-sql/azure-sql-symbol.png)

<span data-ttu-id="edf95-105">Řešení Azure SQL analýzy v Azure Log Analytics shromažďuje a vizualizuje důležité metriky výkonu SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="edf95-105">The Azure SQL Analytics solution in Azure Log Analytics collects and visualizes important SQL Azure performance metrics.</span></span> <span data-ttu-id="edf95-106">Pomocí metriky, které shromažďujete s řešením, můžete vytvořit vlastní pravidla monitorování a výstrahy.</span><span class="sxs-lookup"><span data-stu-id="edf95-106">By using the metrics that you collect with the solution, you can create custom monitoring rules and alerts.</span></span> <span data-ttu-id="edf95-107">A, můžete monitorovat Azure SQL Database a metriky elastického fondu napříč více předplatných Azure a elastického fondu a vizualizovat je.</span><span class="sxs-lookup"><span data-stu-id="edf95-107">And, you can monitor Azure SQL Database and elastic pool metrics across multiple Azure subscriptions and elastic pools and visualize them.</span></span> <span data-ttu-id="edf95-108">Řešení také vám pomůže identifikovat problémy v každé vrstvě zásobníku vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="edf95-108">The solution also helps you to identify issues at each layer of your application stack.</span></span>  <span data-ttu-id="edf95-109">Používá [Azure diagnostiky metriky](log-analytics-azure-storage.md) společně s zobrazení analýzy protokolů pro zobrazení dat o databází Azure SQL a elastické fondy v jedné pracovní prostor analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="edf95-109">It uses [Azure Diagnostic metrics](log-analytics-azure-storage.md) together with Log Analytics views to present data about all your Azure SQL databases and elastic pools in a single Log Analytics workspace.</span></span>

<span data-ttu-id="edf95-110">Toto řešení preview v současné době podporuje až 150 000 databází SQL Azure a 5 000 SQL elastické fondy podle pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="edf95-110">Currently, this preview solution supports up to 150,000 Azure SQL Databases and 5,000 SQL Elastic Pools per workspace.</span></span>

<span data-ttu-id="edf95-111">Řešení Azure SQL Analytics, jako je k dispozici pro analýzy protokolů, ostatní pomáhá sledovat a přijímání oznámení o stavu vašich prostředků Azure – v tomto případě Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="edf95-111">The Azure SQL Analytics solution, like others available for Log Analytics, helps you monitor and receive notifications about the health of your Azure resources—in this case, Azure SQL Database.</span></span> <span data-ttu-id="edf95-112">Microsoft Azure SQL Database je služba škálovatelné relační databáze, který nabízí známé jako SQL Server – funkce pro aplikace běžící v cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="edf95-112">Microsoft Azure SQL Database is a scalable relational database service that provides familiar SQL-Server-like capabilities to applications running in the Azure cloud.</span></span> <span data-ttu-id="edf95-113">Analýzy protokolů umožňuje shromažďovat, korelaci a vizualizovat strukturovaná i nestrukturovaná data.</span><span class="sxs-lookup"><span data-stu-id="edf95-113">Log Analytics helps you to collect, correlate, and visualize structured and unstructured data.</span></span>

## <a name="connected-sources"></a><span data-ttu-id="edf95-114">Připojené zdroje</span><span class="sxs-lookup"><span data-stu-id="edf95-114">Connected sources</span></span>

<span data-ttu-id="edf95-115">Řešení Azure SQL analýzy nepoužívá agentů pro připojení ke službě Analýza protokolů.</span><span class="sxs-lookup"><span data-stu-id="edf95-115">The Azure SQL Analytics solution doesn't use agents to connect to the Log Analytics service.</span></span>

<span data-ttu-id="edf95-116">Následující tabulka popisuje připojené zdroje, které toto řešení podporuje.</span><span class="sxs-lookup"><span data-stu-id="edf95-116">The following table describes the connected sources that are supported by this solution.</span></span>

| <span data-ttu-id="edf95-117">Připojený zdroj</span><span class="sxs-lookup"><span data-stu-id="edf95-117">Connected Source</span></span> | <span data-ttu-id="edf95-118">Podpora</span><span class="sxs-lookup"><span data-stu-id="edf95-118">Support</span></span> | <span data-ttu-id="edf95-119">Popis</span><span class="sxs-lookup"><span data-stu-id="edf95-119">Description</span></span> |
| --- | --- | --- |
| [<span data-ttu-id="edf95-120">Agenti systému Windows</span><span class="sxs-lookup"><span data-stu-id="edf95-120">Windows agents</span></span>](log-analytics-windows-agents.md) | <span data-ttu-id="edf95-121">Ne</span><span class="sxs-lookup"><span data-stu-id="edf95-121">No</span></span> | <span data-ttu-id="edf95-122">Přímé agentů v systému Windows nejsou používány nástrojem řešení.</span><span class="sxs-lookup"><span data-stu-id="edf95-122">Direct Windows agents are not used by the solution.</span></span> |
| [<span data-ttu-id="edf95-123">Agenti systému Linux</span><span class="sxs-lookup"><span data-stu-id="edf95-123">Linux agents</span></span>](log-analytics-linux-agents.md) | <span data-ttu-id="edf95-124">Ne</span><span class="sxs-lookup"><span data-stu-id="edf95-124">No</span></span> | <span data-ttu-id="edf95-125">Přímé agenty Linux nejsou používány nástrojem řešení.</span><span class="sxs-lookup"><span data-stu-id="edf95-125">Direct Linux agents are not used by the solution.</span></span> |
| [<span data-ttu-id="edf95-126">Skupiny správy nástroje SCOM</span><span class="sxs-lookup"><span data-stu-id="edf95-126">SCOM management group</span></span>](log-analytics-om-agents.md) | <span data-ttu-id="edf95-127">Ne</span><span class="sxs-lookup"><span data-stu-id="edf95-127">No</span></span> | <span data-ttu-id="edf95-128">Přímé připojení z agenta nástroje SCOM k analýze protokolů nepoužívá řešení.</span><span class="sxs-lookup"><span data-stu-id="edf95-128">A direct connection from the SCOM agent to Log Analytics is not used by the solution.</span></span> |
| [<span data-ttu-id="edf95-129">Účet služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="edf95-129">Azure storage account</span></span>](log-analytics-azure-storage.md) | <span data-ttu-id="edf95-130">Ne</span><span class="sxs-lookup"><span data-stu-id="edf95-130">No</span></span> | <span data-ttu-id="edf95-131">Analýzy protokolů číst data z účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="edf95-131">Log Analytics does not read the data from a storage account.</span></span> |
| [<span data-ttu-id="edf95-132">Azure diagnostics</span><span class="sxs-lookup"><span data-stu-id="edf95-132">Azure diagnostics</span></span>](log-analytics-azure-storage.md) | <span data-ttu-id="edf95-133">Ano</span><span class="sxs-lookup"><span data-stu-id="edf95-133">Yes</span></span> | <span data-ttu-id="edf95-134">Azure metriky data se odešlou do Log Analytics přímo v Azure.</span><span class="sxs-lookup"><span data-stu-id="edf95-134">Azure metric data is sent to Log Analytics directly by Azure.</span></span> |

## <a name="prerequisites"></a><span data-ttu-id="edf95-135">Požadavky</span><span class="sxs-lookup"><span data-stu-id="edf95-135">Prerequisites</span></span>

- <span data-ttu-id="edf95-136">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="edf95-136">An Azure Subscription.</span></span> <span data-ttu-id="edf95-137">Pokud nemáte, můžete vytvořit jeden pro [volné](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="edf95-137">If you don't have one, you can create one for [free](https://azure.microsoft.com/free/).</span></span>
- <span data-ttu-id="edf95-138">Pracovní prostor analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="edf95-138">A Log Analytics workspace.</span></span> <span data-ttu-id="edf95-139">Můžete použít existující, nebo můžete [vytvořte novou](log-analytics-get-started.md) než začnete používat tohoto řešení.</span><span class="sxs-lookup"><span data-stu-id="edf95-139">You can use an existing one, or you can [create a new one](log-analytics-get-started.md) before you start using this solution.</span></span>
- <span data-ttu-id="edf95-140">Povolit Azure Diagnostics pro databáze Azure SQL a elastické fondy a [nakonfigurovat je pro odesílají data k analýze protokolů](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).</span><span class="sxs-lookup"><span data-stu-id="edf95-140">Enable Azure Diagnostics for your Azure SQL databases and elastic pools and [configure them to send their data to Log Analytics](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).</span></span>

## <a name="configuration"></a><span data-ttu-id="edf95-141">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="edf95-141">Configuration</span></span>

<span data-ttu-id="edf95-142">Proveďte následující postup pro přidání řešení analýzy SQL Azure do pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="edf95-142">Perform the following steps to add the Azure SQL Analytics solution to your workspace.</span></span>

1. <span data-ttu-id="edf95-143">Přidat řešení analýzy SQL Azure do pracovního prostoru z [Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview) nebo pomocí procesu popsaného v tématu [řešení přidat analýzy protokolů z Galerie řešení](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="edf95-143">Add the Azure SQL Analytics solution to your workspace from [Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview) or by using the process described in [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md).</span></span>
2. <span data-ttu-id="edf95-144">Na portálu Azure klikněte na tlačítko **nový** (+ symbol), pak v seznamu prostředků, vyberte **monitorování + správu**.</span><span class="sxs-lookup"><span data-stu-id="edf95-144">In the Azure portal, click **New** (the + symbol), then in the list of resources, select **Monitoring + Management**.</span></span>  
    <span data-ttu-id="edf95-145">![Monitorování a správa](./media/log-analytics-azure-sql/monitoring-management.png)</span><span class="sxs-lookup"><span data-stu-id="edf95-145">![Monitoring + Management](./media/log-analytics-azure-sql/monitoring-management.png)</span></span>
3. <span data-ttu-id="edf95-146">V **monitorování + správu** seznamu klikněte na tlačítko **zobrazit všechny**.</span><span class="sxs-lookup"><span data-stu-id="edf95-146">In the **Monitoring + Management** list click **See all**.</span></span>
4. <span data-ttu-id="edf95-147">V **doporučeno** seznamu, klikněte na tlačítko **Další** a pak v seznamu nové vyhledejte **analýzy SQL Azure (Preview)** a pak ho vyberte.</span><span class="sxs-lookup"><span data-stu-id="edf95-147">In the **Recommended** list, click **More** , and then in the new list, find **Azure SQL Analytics (Preview)** and then select it.</span></span>  
    <span data-ttu-id="edf95-148">![Řešení Azure SQL analýzy](./media/log-analytics-azure-sql/azure-sql-solution-portal.png)</span><span class="sxs-lookup"><span data-stu-id="edf95-148">![Azure SQL Analytics solution](./media/log-analytics-azure-sql/azure-sql-solution-portal.png)</span></span>
5. <span data-ttu-id="edf95-149">V **analýzy SQL Azure (Preview)** okně klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="edf95-149">In the **Azure SQL Analytics (Preview)** blade, click **Create**.</span></span>  
    <span data-ttu-id="edf95-150">![Vytvoření](./media/log-analytics-azure-sql/portal-create.png)</span><span class="sxs-lookup"><span data-stu-id="edf95-150">![Create](./media/log-analytics-azure-sql/portal-create.png)</span></span>
6. <span data-ttu-id="edf95-151">V **vytvořte nové řešení** okně, vyberte pracovní prostor, který chcete přidat řešení a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="edf95-151">In the **Create new solution** blade, select the workspace that you want to add the solution to and then click **Create**.</span></span>  
    <span data-ttu-id="edf95-152">![Přidat do pracovního prostoru](./media/log-analytics-azure-sql/add-to-workspace.png)</span><span class="sxs-lookup"><span data-stu-id="edf95-152">![add to workspace](./media/log-analytics-azure-sql/add-to-workspace.png)</span></span>


### <a name="to-configure-multiple-azure-subscriptions"></a><span data-ttu-id="edf95-153">Ke konfiguraci víc předplatných Azure</span><span class="sxs-lookup"><span data-stu-id="edf95-153">To configure multiple Azure subscriptions</span></span>

<span data-ttu-id="edf95-154">Pro podporu více předplatných, pomocí skriptu prostředí PowerShell z [povolit Azure prostředku metriky protokolování pomocí prostředí PowerShell](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).</span><span class="sxs-lookup"><span data-stu-id="edf95-154">To support multiple subscriptions, use the PowerShell script from [Enable Azure resource metrics logging using PowerShell](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).</span></span> <span data-ttu-id="edf95-155">Při provádění skriptu k odesílání diagnostických dat z prostředků v rámci jednoho předplatného Azure do pracovního prostoru v jiného předplatného Azure, zadejte ID prostředku prostoru jako parametr.</span><span class="sxs-lookup"><span data-stu-id="edf95-155">Provide the workspace resource ID as a parameter when executing the script to send diagnostic data from resources in one Azure subscription to a workspace in another Azure subscription.</span></span>

<span data-ttu-id="edf95-156">**Příklad**</span><span class="sxs-lookup"><span data-stu-id="edf95-156">**Example**</span></span>

```
PS C:\> $WSID = "/subscriptions/<subID>/resourcegroups/oms/providers/microsoft.operationalinsights/workspaces/omsws"
```

```
PS C:\> .\Enable-AzureRMDiagnostics.ps1 -WSID $WSID
```

## <a name="using-the-solution"></a><span data-ttu-id="edf95-157">Použití řešení</span><span class="sxs-lookup"><span data-stu-id="edf95-157">Using the solution</span></span>

<span data-ttu-id="edf95-158">Když přidáte řešení do pracovního prostoru, dlaždici analýzy SQL Azure je přidat do pracovního prostoru, a zobrazí se v přehledu.</span><span class="sxs-lookup"><span data-stu-id="edf95-158">When you add the solution to your workspace, the Azure SQL Analytics tile is added to your workspace, and it appears in Overview.</span></span> <span data-ttu-id="edf95-159">Na dlaždici zobrazuje počet databází Azure SQL a Azure SQL elastické fondy, které řešení je připojen k.</span><span class="sxs-lookup"><span data-stu-id="edf95-159">The tile shows the number of Azure SQL databases and Azure SQL elastic pools that the solution is connected to.</span></span>

![Azure SQL Analytics dlaždice](./media/log-analytics-azure-sql/azure-sql-sol-tile.png)

### <a name="viewing-azure-sql-analytics-data"></a><span data-ttu-id="edf95-161">Zobrazení dat analýzy SQL Azure</span><span class="sxs-lookup"><span data-stu-id="edf95-161">Viewing Azure SQL Analytics data</span></span>

<span data-ttu-id="edf95-162">Klikněte na **analýzy SQL Azure** dlaždici otevřete řídicí panel Azure SQL Analytics.</span><span class="sxs-lookup"><span data-stu-id="edf95-162">Click on the **Azure SQL Analytics** tile to open the Azure SQL Analytics dashboard.</span></span> <span data-ttu-id="edf95-163">Řídicí panel obsahuje okna definovaná níže.</span><span class="sxs-lookup"><span data-stu-id="edf95-163">The dashboard includes the blades defined below.</span></span> <span data-ttu-id="edf95-164">Každý okno seznam až 15 prostředků (předplatné, server, elastického fondu a databáze).</span><span class="sxs-lookup"><span data-stu-id="edf95-164">Each blade lists up to 15 resources (subscription, server, elastic pool, and database).</span></span> <span data-ttu-id="edf95-165">Klikněte na některý ze zdrojů otevřete řídící panel pro tento konkrétní prostředek.</span><span class="sxs-lookup"><span data-stu-id="edf95-165">Click any of the resources to open the dashboard for that specific resource.</span></span> <span data-ttu-id="edf95-166">Elastický fond nebo databáze obsahuje grafy s metriky pro vybraný zdroj.</span><span class="sxs-lookup"><span data-stu-id="edf95-166">Elastic Pool or Database contains the charts with metrics for a selected resource.</span></span> <span data-ttu-id="edf95-167">Klikněte na graf a otevřete dialogové okno hledání protokolů.</span><span class="sxs-lookup"><span data-stu-id="edf95-167">Click a chart to open the Log Search dialog.</span></span>

| <span data-ttu-id="edf95-168">Okno</span><span class="sxs-lookup"><span data-stu-id="edf95-168">Blade</span></span> | <span data-ttu-id="edf95-169">Popis</span><span class="sxs-lookup"><span data-stu-id="edf95-169">Description</span></span> |
|---|---|
| <span data-ttu-id="edf95-170">Předplatná</span><span class="sxs-lookup"><span data-stu-id="edf95-170">Subscriptions</span></span> | <span data-ttu-id="edf95-171">Seznam předplatných s počtem propojené servery, fondy a databáze.</span><span class="sxs-lookup"><span data-stu-id="edf95-171">List of subscriptions with number of connected servers, pools, and databases.</span></span> |
| <span data-ttu-id="edf95-172">Servery</span><span class="sxs-lookup"><span data-stu-id="edf95-172">Servers</span></span> | <span data-ttu-id="edf95-173">Seznam serverů s počtem připojené fondy a databází.</span><span class="sxs-lookup"><span data-stu-id="edf95-173">List of servers with number of connected pools and databases.</span></span> |
| <span data-ttu-id="edf95-174">Elastické fondy</span><span class="sxs-lookup"><span data-stu-id="edf95-174">Elastic Pools</span></span> | <span data-ttu-id="edf95-175">Seznam připojených elastické fondy s maximální GB a eDTU v zjištěnou období.</span><span class="sxs-lookup"><span data-stu-id="edf95-175">List of connected elastic pools with maximum GB and eDTU in the observed period.</span></span> |
|<span data-ttu-id="edf95-176">Databáze</span><span class="sxs-lookup"><span data-stu-id="edf95-176">Databases</span></span> | <span data-ttu-id="edf95-177">Seznam připojených databází s maximální GB a DTU v zjištěnou období.</span><span class="sxs-lookup"><span data-stu-id="edf95-177">List of connected databases with maximum GB and DTU in the observed period.</span></span>|


### <a name="analyze-data-and-create-alerts"></a><span data-ttu-id="edf95-178">Analyzovat data a vytvářet výstrahy</span><span class="sxs-lookup"><span data-stu-id="edf95-178">Analyze data and create alerts</span></span>

<span data-ttu-id="edf95-179">Výstrahy můžete snadno vytvořit s dat pocházejících z prostředků Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="edf95-179">You can easily create alerts with the data coming from Azure SQL Database resources.</span></span> <span data-ttu-id="edf95-180">Tady je několik užitečné [hledání protokolů](log-analytics-log-searches.md) dotazy, které můžete použít pro výstrahy:</span><span class="sxs-lookup"><span data-stu-id="edf95-180">Here are a couple of useful [log search](log-analytics-log-searches.md) queries that you can use for alerting:</span></span>

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]


<span data-ttu-id="edf95-181">*Vysoký počet jednotek DTU na databázi Azure SQL*</span><span class="sxs-lookup"><span data-stu-id="edf95-181">*High DTU on Azure SQL Database*</span></span>

```
Type=AzureMetrics ResourceProvider="MICROSOFT.SQL" ResourceId=*"/DATABASES/"* MetricName=dtu_consumption_percent | measure Avg(Average) by Resource interval 5minutes
```

<span data-ttu-id="edf95-182">*Vysoký počet jednotek DTU na fond elastické databáze Azure SQL*</span><span class="sxs-lookup"><span data-stu-id="edf95-182">*High DTU on Azure SQL Database Elastic Pool*</span></span>

```
Type=AzureMetrics ResourceProvider="MICROSOFT.SQL" ResourceId=*"/ELASTICPOOLS/"* MetricName=dtu_consumption_percent | measure avg(Average) by Resource interval 5minutes
```

<span data-ttu-id="edf95-183">Tyto dotazy na základě výstrahy můžete výstrahy na specifické prahové hodnoty pro Azure SQL Database a elastické fondy.</span><span class="sxs-lookup"><span data-stu-id="edf95-183">You can use these alert-based queries to alert on specific thresholds for both Azure SQL Database and elastic pools.</span></span> <span data-ttu-id="edf95-184">Konfigurace oznámení pro pracovní prostor OMS:</span><span class="sxs-lookup"><span data-stu-id="edf95-184">To configure an alert for your OMS workspace:</span></span>

#### <a name="to-configure-an-alert-for-your-workspace"></a><span data-ttu-id="edf95-185">Konfigurace oznámení pro pracovní prostor</span><span class="sxs-lookup"><span data-stu-id="edf95-185">To configure an alert for your workspace</span></span>

1. <span data-ttu-id="edf95-186">Přejděte na [portálu OMS](http://mms.microsoft.com/) a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="edf95-186">Go to the [OMS portal](http://mms.microsoft.com/) and sign in.</span></span>
2. <span data-ttu-id="edf95-187">Otevřete pracovní prostor, který jste nakonfigurovali pro řešení.</span><span class="sxs-lookup"><span data-stu-id="edf95-187">Open the workspace that you have configured for the solution.</span></span>
3. <span data-ttu-id="edf95-188">Na stránce Přehled klikněte na **analýzy SQL Azure (Preview)** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="edf95-188">On the Overview page, click the **Azure SQL Analytics (Preview)** tile.</span></span>
4. <span data-ttu-id="edf95-189">Spusťte jeden z příkladů dotazů.</span><span class="sxs-lookup"><span data-stu-id="edf95-189">Run one of the example queries.</span></span>
5. <span data-ttu-id="edf95-190">V protokolu hledání, klikněte na **výstrahy**.</span><span class="sxs-lookup"><span data-stu-id="edf95-190">In Log Search, click **Alert**.</span></span>  
<span data-ttu-id="edf95-191">![Vytvořit výstrahu pro vyhledávání](./media/log-analytics-azure-sql/create-alert01.png)</span><span class="sxs-lookup"><span data-stu-id="edf95-191">![create alert in search](./media/log-analytics-azure-sql/create-alert01.png)</span></span>
6. <span data-ttu-id="edf95-192">Na **přidat pravidlo výstrahy** nakonfigurujte příslušné vlastnosti a specifické prahové hodnoty, které chcete a pak klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="edf95-192">On the **Add Alert Rule** page, configure the appropriate properties and the specific thresholds that you want and then click **Save**.</span></span>  
<span data-ttu-id="edf95-193">![Přidání pravidla výstrahy](./media/log-analytics-azure-sql/create-alert02.png)</span><span class="sxs-lookup"><span data-stu-id="edf95-193">![add alert rule](./media/log-analytics-azure-sql/create-alert02.png)</span></span>

### <a name="act-on-azure-sql-analytics-data"></a><span data-ttu-id="edf95-194">Fungují v Azure SQL analytická data</span><span class="sxs-lookup"><span data-stu-id="edf95-194">Act on Azure SQL Analytics data</span></span>

<span data-ttu-id="edf95-195">Jako příklad jedním nejužitečnější dotazů, které můžete provádět je porovnání využití DTU pro všechny fondy elastické SQL Azure ve všech vašich předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="edf95-195">As an example, one of the most useful queries that you can perform is to compare the DTU utilization for all Azure SQL Elastic Pools across all your Azure subscriptions.</span></span> <span data-ttu-id="edf95-196">Jednotky propustnosti databáze (DTU) poskytuje způsob, jak popisují relativní kapacitu úrovně výkonu databází Basic, Standard a Premium a fondy.</span><span class="sxs-lookup"><span data-stu-id="edf95-196">Database Throughput Unit (DTU) provides a way to describe the relative capacity of a performance level of Basic, Standard, and Premium databases and pools.</span></span> <span data-ttu-id="edf95-197">Počet jednotek Dtu jsou založená na kombinaci měření procesoru, paměti, čte a zapisuje.</span><span class="sxs-lookup"><span data-stu-id="edf95-197">DTUs are based on a blended measure of CPU, memory, reads, and writes.</span></span> <span data-ttu-id="edf95-198">Jak zvýšit počet jednotek Dtu, zvyšuje napájení, které nabízí úroveň výkonu.</span><span class="sxs-lookup"><span data-stu-id="edf95-198">As DTUs increase, the power offered by the performance level increases.</span></span> <span data-ttu-id="edf95-199">Například úroveň výkonu s 5 Dtu má pětkrát další výkon než úroveň výkonu s 1 DTU.</span><span class="sxs-lookup"><span data-stu-id="edf95-199">For example, a performance level with 5 DTUs has five times more power than a performance level with 1 DTU.</span></span> <span data-ttu-id="edf95-200">Maximální kvóty DTU se vztahuje na každém serveru a elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="edf95-200">A maximum DTU quota applies to each server and elastic pool.</span></span>

<span data-ttu-id="edf95-201">Spuštěním následujícího dotazu hledání protokolů můžete snadno určit, pokud jsou nedostatečně využívá nebo přes využitím fondech elastické SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="edf95-201">By running the following Log Search query, you can easily tell if you are underutilizing or over utilizing your SQL Azure elastic pools.</span></span>

```
Type=AzureMetrics ResourceId=*"/ELASTICPOOLS/"* MetricName=dtu_consumption_percent | measure avg(Average) by Resource | display LineChart
```

>[!NOTE]
> <span data-ttu-id="edf95-202">Pokud pracovní prostor byl upgradován na verzi [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak výše uvedeném dotazu by změnit na následující.</span><span class="sxs-lookup"><span data-stu-id="edf95-202">If your workspace has been upgraded to the [new Log Analytics query language](log-analytics-log-search-upgrade.md), then the above query would change to the following.</span></span>
>
>`search in (AzureMetrics) isnotempty(ResourceId) and "/ELASTICPOOLS/" and MetricName == "dtu_consumption_percent" | summarize AggregatedValue = avg(Average) by bin(TimeGenerated, 1h), Resource | render timechart`

<span data-ttu-id="edf95-203">V následujícím příkladu vidíte, že jeden elastického fondu má vysoké využití téměř 100 % DTU jiné mají velmi malé využití.</span><span class="sxs-lookup"><span data-stu-id="edf95-203">In the following example, you can see that one elastic pool has a high usage near 100% DTU while others have very little usage.</span></span> <span data-ttu-id="edf95-204">Můžete prozkoumat další řešení potenciální dopad nedávných změn ve vašem prostředí, pomocí protokolů aktivita služby Azure.</span><span class="sxs-lookup"><span data-stu-id="edf95-204">You can investigate further to troubleshoot potential recent changes in your environment using Azure Activity logs.</span></span>

![Výsledky hledání protokolu - vysoké využití](./media/log-analytics-azure-sql/log-search-high-util.png)

## <a name="see-also"></a><span data-ttu-id="edf95-206">Viz také</span><span class="sxs-lookup"><span data-stu-id="edf95-206">See also</span></span>

- <span data-ttu-id="edf95-207">Použití [protokolu hledání](log-analytics-log-searches.md) v analýzy protokolů, chcete-li zobrazit podrobné dat Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="edf95-207">Use [Log Searches](log-analytics-log-searches.md) in Log Analytics to view detailed Azure SQL data.</span></span>
- <span data-ttu-id="edf95-208">[Vytvořit vlastní řídicí panely](log-analytics-dashboards.md) zobrazující dat Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="edf95-208">[Create your own dashboards](log-analytics-dashboards.md) showing Azure SQL data.</span></span>
- <span data-ttu-id="edf95-209">[Vytvářet výstrahy](log-analytics-alerts.md) při výskytu určité události Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="edf95-209">[Create alerts](log-analytics-alerts.md) when specific Azure SQL events occur.</span></span>
