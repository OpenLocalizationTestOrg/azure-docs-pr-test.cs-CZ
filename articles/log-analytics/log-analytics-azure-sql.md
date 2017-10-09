---
title: "aaaAzure řešení SQL analýzy v Log Analytics | Microsoft Docs"
description: "Hello řešení analýzy SQL Azure pomáhá spravovat vaše databáze Azure SQL."
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
ms.openlocfilehash: fe228bb3cb3f9d578a84707c3917f02fbeb8a627
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-azure-sql-database-using-azure-sql-analytics-preview-in-log-analytics"></a><span data-ttu-id="53578-103">Monitorování databáze Azure SQL pomocí analýzy SQL Azure (Preview) v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="53578-103">Monitor Azure SQL Database using Azure SQL Analytics (Preview) in Log Analytics</span></span>

![Azure SQL Analytics symbol](./media/log-analytics-azure-sql/azure-sql-symbol.png)

<span data-ttu-id="53578-105">Hello řešení analýzy SQL Azure v Azure Log Analytics shromažďuje a vizualizuje důležité metriky výkonu SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="53578-105">hello Azure SQL Analytics solution in Azure Log Analytics collects and visualizes important SQL Azure performance metrics.</span></span> <span data-ttu-id="53578-106">Pomocí hello metriky, které shromažďujete hello řešení, můžete vytvořit vlastní pravidla monitorování a výstrahy.</span><span class="sxs-lookup"><span data-stu-id="53578-106">By using hello metrics that you collect with hello solution, you can create custom monitoring rules and alerts.</span></span> <span data-ttu-id="53578-107">A, můžete monitorovat Azure SQL Database a metriky elastického fondu napříč více předplatných Azure a elastického fondu a vizualizovat je.</span><span class="sxs-lookup"><span data-stu-id="53578-107">And, you can monitor Azure SQL Database and elastic pool metrics across multiple Azure subscriptions and elastic pools and visualize them.</span></span> <span data-ttu-id="53578-108">řešení Hello také pomáhá tooidentify problémy v každé vrstvě zásobníku vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="53578-108">hello solution also helps you tooidentify issues at each layer of your application stack.</span></span>  <span data-ttu-id="53578-109">Používá [Azure diagnostiky metriky](log-analytics-azure-storage.md) společně s analýzy protokolů zobrazení toopresent data o databází Azure SQL a elastické fondy v jedné pracovní prostor analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="53578-109">It uses [Azure Diagnostic metrics](log-analytics-azure-storage.md) together with Log Analytics views toopresent data about all your Azure SQL databases and elastic pools in a single Log Analytics workspace.</span></span>

<span data-ttu-id="53578-110">Toto řešení preview v současné době podporuje až too150 000 databází SQL Azure a 5 000 SQL elastické fondy podle pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="53578-110">Currently, this preview solution supports up too150,000 Azure SQL Databases and 5,000 SQL Elastic Pools per workspace.</span></span>

<span data-ttu-id="53578-111">Hello řešení analýzy SQL Azure, jako je k dispozici pro analýzy protokolů, ostatní pomáhá sledovat a přijímat oznámení o stavu hello vašich prostředků Azure – v tomto případě Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="53578-111">hello Azure SQL Analytics solution, like others available for Log Analytics, helps you monitor and receive notifications about hello health of your Azure resources—in this case, Azure SQL Database.</span></span> <span data-ttu-id="53578-112">Microsoft Azure SQL Database je služba škálovatelné relační databáze, která poskytuje známé tooapplications možnosti SQL. Server jako spuštěn v hello cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="53578-112">Microsoft Azure SQL Database is a scalable relational database service that provides familiar SQL-Server-like capabilities tooapplications running in hello Azure cloud.</span></span> <span data-ttu-id="53578-113">Analýzy protokolů vám pomůže toocollect, korelaci a vizualizovat strukturovaná i nestrukturovaná data.</span><span class="sxs-lookup"><span data-stu-id="53578-113">Log Analytics helps you toocollect, correlate, and visualize structured and unstructured data.</span></span>

## <a name="connected-sources"></a><span data-ttu-id="53578-114">Připojené zdroje</span><span class="sxs-lookup"><span data-stu-id="53578-114">Connected sources</span></span>

<span data-ttu-id="53578-115">Hello řešení analýzy SQL Azure nepoužívá agenty tooconnect toohello analýzy protokolů služby.</span><span class="sxs-lookup"><span data-stu-id="53578-115">hello Azure SQL Analytics solution doesn't use agents tooconnect toohello Log Analytics service.</span></span>

<span data-ttu-id="53578-116">Hello následující tabulka popisuje hello připojené zdroje, které podporuje toto řešení.</span><span class="sxs-lookup"><span data-stu-id="53578-116">hello following table describes hello connected sources that are supported by this solution.</span></span>

| <span data-ttu-id="53578-117">Připojený zdroj</span><span class="sxs-lookup"><span data-stu-id="53578-117">Connected Source</span></span> | <span data-ttu-id="53578-118">Podpora</span><span class="sxs-lookup"><span data-stu-id="53578-118">Support</span></span> | <span data-ttu-id="53578-119">Popis</span><span class="sxs-lookup"><span data-stu-id="53578-119">Description</span></span> |
| --- | --- | --- |
| [<span data-ttu-id="53578-120">Agenti systému Windows</span><span class="sxs-lookup"><span data-stu-id="53578-120">Windows agents</span></span>](log-analytics-windows-agents.md) | <span data-ttu-id="53578-121">Ne</span><span class="sxs-lookup"><span data-stu-id="53578-121">No</span></span> | <span data-ttu-id="53578-122">Přímé agentů v systému Windows nejsou používány nástrojem hello řešení.</span><span class="sxs-lookup"><span data-stu-id="53578-122">Direct Windows agents are not used by hello solution.</span></span> |
| [<span data-ttu-id="53578-123">Agenti systému Linux</span><span class="sxs-lookup"><span data-stu-id="53578-123">Linux agents</span></span>](log-analytics-linux-agents.md) | <span data-ttu-id="53578-124">Ne</span><span class="sxs-lookup"><span data-stu-id="53578-124">No</span></span> | <span data-ttu-id="53578-125">Přímé agenty Linux nejsou používány nástrojem hello řešení.</span><span class="sxs-lookup"><span data-stu-id="53578-125">Direct Linux agents are not used by hello solution.</span></span> |
| [<span data-ttu-id="53578-126">Skupiny správy nástroje SCOM</span><span class="sxs-lookup"><span data-stu-id="53578-126">SCOM management group</span></span>](log-analytics-om-agents.md) | <span data-ttu-id="53578-127">Ne</span><span class="sxs-lookup"><span data-stu-id="53578-127">No</span></span> | <span data-ttu-id="53578-128">Přímé připojení z hello SCOM agenta tooLog Analytics není používán hello řešení.</span><span class="sxs-lookup"><span data-stu-id="53578-128">A direct connection from hello SCOM agent tooLog Analytics is not used by hello solution.</span></span> |
| [<span data-ttu-id="53578-129">Účet služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="53578-129">Azure storage account</span></span>](log-analytics-azure-storage.md) | <span data-ttu-id="53578-130">Ne</span><span class="sxs-lookup"><span data-stu-id="53578-130">No</span></span> | <span data-ttu-id="53578-131">Analýzy protokolů není přečíst hello data z účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="53578-131">Log Analytics does not read hello data from a storage account.</span></span> |
| [<span data-ttu-id="53578-132">Azure diagnostics</span><span class="sxs-lookup"><span data-stu-id="53578-132">Azure diagnostics</span></span>](log-analytics-azure-storage.md) | <span data-ttu-id="53578-133">Ano</span><span class="sxs-lookup"><span data-stu-id="53578-133">Yes</span></span> | <span data-ttu-id="53578-134">Azure metriky data se odešlou tooLog Analytics přímo v Azure.</span><span class="sxs-lookup"><span data-stu-id="53578-134">Azure metric data is sent tooLog Analytics directly by Azure.</span></span> |

## <a name="prerequisites"></a><span data-ttu-id="53578-135">Požadavky</span><span class="sxs-lookup"><span data-stu-id="53578-135">Prerequisites</span></span>

- <span data-ttu-id="53578-136">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="53578-136">An Azure Subscription.</span></span> <span data-ttu-id="53578-137">Pokud nemáte, můžete vytvořit jeden pro [volné](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="53578-137">If you don't have one, you can create one for [free](https://azure.microsoft.com/free/).</span></span>
- <span data-ttu-id="53578-138">Pracovní prostor analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="53578-138">A Log Analytics workspace.</span></span> <span data-ttu-id="53578-139">Můžete použít existující, nebo můžete [vytvořte novou](log-analytics-get-started.md) než začnete používat tohoto řešení.</span><span class="sxs-lookup"><span data-stu-id="53578-139">You can use an existing one, or you can [create a new one](log-analytics-get-started.md) before you start using this solution.</span></span>
- <span data-ttu-id="53578-140">Povolit Azure Diagnostics pro databáze Azure SQL a elastické fondy a [je nakonfigurovat toosend jejich data tooLog Analytics](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).</span><span class="sxs-lookup"><span data-stu-id="53578-140">Enable Azure Diagnostics for your Azure SQL databases and elastic pools and [configure them toosend their data tooLog Analytics](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).</span></span>

## <a name="configuration"></a><span data-ttu-id="53578-141">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="53578-141">Configuration</span></span>

<span data-ttu-id="53578-142">Proveďte následující kroky tooadd hello Azure SQL řešení tooyour pracovní prostor analýzy hello.</span><span class="sxs-lookup"><span data-stu-id="53578-142">Perform hello following steps tooadd hello Azure SQL Analytics solution tooyour workspace.</span></span>

1. <span data-ttu-id="53578-143">Přidat hello pracovní prostor tooyour řešení analýzy SQL Azure z [Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview) nebo pomocí hello procesu popsaného v tématu [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="53578-143">Add hello Azure SQL Analytics solution tooyour workspace from [Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview) or by using hello process described in [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md).</span></span>
2. <span data-ttu-id="53578-144">V hello portálu Azure, klikněte na **nový** (hello + symbol), pak v seznamu hello prostředků, vyberte **monitorování + správu**.</span><span class="sxs-lookup"><span data-stu-id="53578-144">In hello Azure portal, click **New** (hello + symbol), then in hello list of resources, select **Monitoring + Management**.</span></span>  
    <span data-ttu-id="53578-145">![Monitorování a správa](./media/log-analytics-azure-sql/monitoring-management.png)</span><span class="sxs-lookup"><span data-stu-id="53578-145">![Monitoring + Management](./media/log-analytics-azure-sql/monitoring-management.png)</span></span>
3. <span data-ttu-id="53578-146">V hello **monitorování + správu** seznamu klikněte na tlačítko **zobrazit všechny**.</span><span class="sxs-lookup"><span data-stu-id="53578-146">In hello **Monitoring + Management** list click **See all**.</span></span>
4. <span data-ttu-id="53578-147">V hello **doporučeno** seznamu, klikněte na tlačítko **Další** a potom v seznamu hello nové, vyhledáte **analýzy SQL Azure (Preview)** a pak ho vyberte.</span><span class="sxs-lookup"><span data-stu-id="53578-147">In hello **Recommended** list, click **More** , and then in hello new list, find **Azure SQL Analytics (Preview)** and then select it.</span></span>  
    <span data-ttu-id="53578-148">![Řešení Azure SQL analýzy](./media/log-analytics-azure-sql/azure-sql-solution-portal.png)</span><span class="sxs-lookup"><span data-stu-id="53578-148">![Azure SQL Analytics solution](./media/log-analytics-azure-sql/azure-sql-solution-portal.png)</span></span>
5. <span data-ttu-id="53578-149">V hello **analýzy SQL Azure (Preview)** okně klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="53578-149">In hello **Azure SQL Analytics (Preview)** blade, click **Create**.</span></span>  
    <span data-ttu-id="53578-150">![Vytvoření](./media/log-analytics-azure-sql/portal-create.png)</span><span class="sxs-lookup"><span data-stu-id="53578-150">![Create](./media/log-analytics-azure-sql/portal-create.png)</span></span>
6. <span data-ttu-id="53578-151">V hello **vytvořte nové řešení** okně, vyberte hello prostoru má tooadd hello řešení tooand a klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="53578-151">In hello **Create new solution** blade, select hello workspace that you want tooadd hello solution tooand then click **Create**.</span></span>  
    <span data-ttu-id="53578-152">![Přidat tooworkspace](./media/log-analytics-azure-sql/add-to-workspace.png)</span><span class="sxs-lookup"><span data-stu-id="53578-152">![add tooworkspace](./media/log-analytics-azure-sql/add-to-workspace.png)</span></span>


### <a name="tooconfigure-multiple-azure-subscriptions"></a><span data-ttu-id="53578-153">tooconfigure víc předplatných Azure</span><span class="sxs-lookup"><span data-stu-id="53578-153">tooconfigure multiple Azure subscriptions</span></span>

<span data-ttu-id="53578-154">toosupport více předplatných, pomocí skriptu prostředí PowerShell hello z [povolit Azure prostředku metriky protokolování pomocí prostředí PowerShell](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).</span><span class="sxs-lookup"><span data-stu-id="53578-154">toosupport multiple subscriptions, use hello PowerShell script from [Enable Azure resource metrics logging using PowerShell](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).</span></span> <span data-ttu-id="53578-155">Při provádění hello skriptu toosend diagnostických dat z prostředků v prostoru tooa jedno předplatné jiného předplatného Azure, zadejte ID prostředku prostoru hello jako parametr.</span><span class="sxs-lookup"><span data-stu-id="53578-155">Provide hello workspace resource ID as a parameter when executing hello script toosend diagnostic data from resources in one Azure subscription tooa workspace in another Azure subscription.</span></span>

<span data-ttu-id="53578-156">**Příklad**</span><span class="sxs-lookup"><span data-stu-id="53578-156">**Example**</span></span>

```
PS C:\> $WSID = "/subscriptions/<subID>/resourcegroups/oms/providers/microsoft.operationalinsights/workspaces/omsws"
```

```
PS C:\> .\Enable-AzureRMDiagnostics.ps1 -WSID $WSID
```

## <a name="using-hello-solution"></a><span data-ttu-id="53578-157">Pomocí řešení hello</span><span class="sxs-lookup"><span data-stu-id="53578-157">Using hello solution</span></span>

<span data-ttu-id="53578-158">Když přidáte hello řešení tooyour prostoru, hello dlaždici analýzy SQL Azure je přidána tooyour prostoru a zobrazí se v přehledu.</span><span class="sxs-lookup"><span data-stu-id="53578-158">When you add hello solution tooyour workspace, hello Azure SQL Analytics tile is added tooyour workspace, and it appears in Overview.</span></span> <span data-ttu-id="53578-159">dlaždice Hello znázorňuje hello počet databází Azure SQL a Azure SQL elastické fondy, které řešení hello je připojen k.</span><span class="sxs-lookup"><span data-stu-id="53578-159">hello tile shows hello number of Azure SQL databases and Azure SQL elastic pools that hello solution is connected to.</span></span>

![Azure SQL Analytics dlaždice](./media/log-analytics-azure-sql/azure-sql-sol-tile.png)

### <a name="viewing-azure-sql-analytics-data"></a><span data-ttu-id="53578-161">Zobrazení dat analýzy SQL Azure</span><span class="sxs-lookup"><span data-stu-id="53578-161">Viewing Azure SQL Analytics data</span></span>

<span data-ttu-id="53578-162">Klikněte na hello **Azure SQL Analytics** řídicí panel dlaždice tooopen hello analýzy SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="53578-162">Click on hello **Azure SQL Analytics** tile tooopen hello Azure SQL Analytics dashboard.</span></span> <span data-ttu-id="53578-163">řídicí panel Hello zahrnuje hello okna definovaná níže.</span><span class="sxs-lookup"><span data-stu-id="53578-163">hello dashboard includes hello blades defined below.</span></span> <span data-ttu-id="53578-164">Každý okno uvádí systémové prostředky too15 (předplatné, server, elastického fondu a databáze).</span><span class="sxs-lookup"><span data-stu-id="53578-164">Each blade lists up too15 resources (subscription, server, elastic pool, and database).</span></span> <span data-ttu-id="53578-165">Klikněte na jakékoliv hello řídicí panel hello tooopen prostředky pro tento konkrétní prostředek.</span><span class="sxs-lookup"><span data-stu-id="53578-165">Click any of hello resources tooopen hello dashboard for that specific resource.</span></span> <span data-ttu-id="53578-166">Elastický fond nebo databáze obsahuje hello grafy s metriky pro vybraný zdroj.</span><span class="sxs-lookup"><span data-stu-id="53578-166">Elastic Pool or Database contains hello charts with metrics for a selected resource.</span></span> <span data-ttu-id="53578-167">Kliknutím na dialogové okno hledání protokolů hello tooopen grafu.</span><span class="sxs-lookup"><span data-stu-id="53578-167">Click a chart tooopen hello Log Search dialog.</span></span>

| <span data-ttu-id="53578-168">Okno</span><span class="sxs-lookup"><span data-stu-id="53578-168">Blade</span></span> | <span data-ttu-id="53578-169">Popis</span><span class="sxs-lookup"><span data-stu-id="53578-169">Description</span></span> |
|---|---|
| <span data-ttu-id="53578-170">Předplatná</span><span class="sxs-lookup"><span data-stu-id="53578-170">Subscriptions</span></span> | <span data-ttu-id="53578-171">Seznam předplatných s počtem propojené servery, fondy a databáze.</span><span class="sxs-lookup"><span data-stu-id="53578-171">List of subscriptions with number of connected servers, pools, and databases.</span></span> |
| <span data-ttu-id="53578-172">Servery</span><span class="sxs-lookup"><span data-stu-id="53578-172">Servers</span></span> | <span data-ttu-id="53578-173">Seznam serverů s počtem připojené fondy a databází.</span><span class="sxs-lookup"><span data-stu-id="53578-173">List of servers with number of connected pools and databases.</span></span> |
| <span data-ttu-id="53578-174">Elastické fondy</span><span class="sxs-lookup"><span data-stu-id="53578-174">Elastic Pools</span></span> | <span data-ttu-id="53578-175">Seznam připojených elastické fondy s maximální GB a eDTU v hello zjištěnými období.</span><span class="sxs-lookup"><span data-stu-id="53578-175">List of connected elastic pools with maximum GB and eDTU in hello observed period.</span></span> |
|<span data-ttu-id="53578-176">Databáze</span><span class="sxs-lookup"><span data-stu-id="53578-176">Databases</span></span> | <span data-ttu-id="53578-177">Seznam připojených databází s maximální GB a DTU v hello zjištěnými období.</span><span class="sxs-lookup"><span data-stu-id="53578-177">List of connected databases with maximum GB and DTU in hello observed period.</span></span>|


### <a name="analyze-data-and-create-alerts"></a><span data-ttu-id="53578-178">Analyzovat data a vytvářet výstrahy</span><span class="sxs-lookup"><span data-stu-id="53578-178">Analyze data and create alerts</span></span>

<span data-ttu-id="53578-179">Výstrahy můžete snadno vytvořit s hello dat pocházejících z prostředků Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="53578-179">You can easily create alerts with hello data coming from Azure SQL Database resources.</span></span> <span data-ttu-id="53578-180">Tady je několik užitečné [hledání protokolů](log-analytics-log-searches.md) dotazy, které můžete použít pro výstrahy:</span><span class="sxs-lookup"><span data-stu-id="53578-180">Here are a couple of useful [log search](log-analytics-log-searches.md) queries that you can use for alerting:</span></span>

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]


<span data-ttu-id="53578-181">*Vysoký počet jednotek DTU na databázi Azure SQL*</span><span class="sxs-lookup"><span data-stu-id="53578-181">*High DTU on Azure SQL Database*</span></span>

```
Type=AzureMetrics ResourceProvider="MICROSOFT.SQL" ResourceId=*"/DATABASES/"* MetricName=dtu_consumption_percent | measure Avg(Average) by Resource interval 5minutes
```

<span data-ttu-id="53578-182">*Vysoký počet jednotek DTU na fond elastické databáze Azure SQL*</span><span class="sxs-lookup"><span data-stu-id="53578-182">*High DTU on Azure SQL Database Elastic Pool*</span></span>

```
Type=AzureMetrics ResourceProvider="MICROSOFT.SQL" ResourceId=*"/ELASTICPOOLS/"* MetricName=dtu_consumption_percent | measure avg(Average) by Resource interval 5minutes
```

<span data-ttu-id="53578-183">Můžete použít tyto dotazy založené na výstrahu tooalert na specifické prahové hodnoty pro Azure SQL Database a elastické fondy.</span><span class="sxs-lookup"><span data-stu-id="53578-183">You can use these alert-based queries tooalert on specific thresholds for both Azure SQL Database and elastic pools.</span></span> <span data-ttu-id="53578-184">tooconfigure oznámení pro pracovní prostor OMS:</span><span class="sxs-lookup"><span data-stu-id="53578-184">tooconfigure an alert for your OMS workspace:</span></span>

#### <a name="tooconfigure-an-alert-for-your-workspace"></a><span data-ttu-id="53578-185">tooconfigure výstrahu vašeho pracovního prostoru</span><span class="sxs-lookup"><span data-stu-id="53578-185">tooconfigure an alert for your workspace</span></span>

1. <span data-ttu-id="53578-186">Přejděte toohello [portálu OMS](http://mms.microsoft.com/) a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="53578-186">Go toohello [OMS portal](http://mms.microsoft.com/) and sign in.</span></span>
2. <span data-ttu-id="53578-187">Otevřete pracovní prostor hello, který jste nakonfigurovali pro řešení hello.</span><span class="sxs-lookup"><span data-stu-id="53578-187">Open hello workspace that you have configured for hello solution.</span></span>
3. <span data-ttu-id="53578-188">Na stránce Přehled hello klikněte hello **analýzy SQL Azure (Preview)** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="53578-188">On hello Overview page, click hello **Azure SQL Analytics (Preview)** tile.</span></span>
4. <span data-ttu-id="53578-189">Spusťte jeden z hello ukázky dotazů.</span><span class="sxs-lookup"><span data-stu-id="53578-189">Run one of hello example queries.</span></span>
5. <span data-ttu-id="53578-190">V protokolu hledání, klikněte na **výstrahy**.</span><span class="sxs-lookup"><span data-stu-id="53578-190">In Log Search, click **Alert**.</span></span>  
<span data-ttu-id="53578-191">![Vytvořit výstrahu pro vyhledávání](./media/log-analytics-azure-sql/create-alert01.png)</span><span class="sxs-lookup"><span data-stu-id="53578-191">![create alert in search](./media/log-analytics-azure-sql/create-alert01.png)</span></span>
6. <span data-ttu-id="53578-192">Na hello **přidat pravidlo výstrahy** nakonfigurujte příslušné vlastnosti hello a hello specifické prahové hodnoty, které chcete a pak klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="53578-192">On hello **Add Alert Rule** page, configure hello appropriate properties and hello specific thresholds that you want and then click **Save**.</span></span>  
<span data-ttu-id="53578-193">![Přidání pravidla výstrahy](./media/log-analytics-azure-sql/create-alert02.png)</span><span class="sxs-lookup"><span data-stu-id="53578-193">![add alert rule](./media/log-analytics-azure-sql/create-alert02.png)</span></span>

### <a name="act-on-azure-sql-analytics-data"></a><span data-ttu-id="53578-194">Fungují v Azure SQL analytická data</span><span class="sxs-lookup"><span data-stu-id="53578-194">Act on Azure SQL Analytics data</span></span>

<span data-ttu-id="53578-195">Například jeden z hello nejužitečnější dotazy, které můžete provádět toocompare hello využití DTU pro všechny fondy elastické SQL Azure je ve všech vašich předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="53578-195">As an example, one of hello most useful queries that you can perform is toocompare hello DTU utilization for all Azure SQL Elastic Pools across all your Azure subscriptions.</span></span> <span data-ttu-id="53578-196">Jednotky propustnosti databáze (DTU) poskytuje způsob toodescribe hello relativní kapacitu úrovně výkonu databází Basic, Standard a Premium a fondy.</span><span class="sxs-lookup"><span data-stu-id="53578-196">Database Throughput Unit (DTU) provides a way toodescribe hello relative capacity of a performance level of Basic, Standard, and Premium databases and pools.</span></span> <span data-ttu-id="53578-197">Počet jednotek Dtu jsou založená na kombinaci měření procesoru, paměti, čte a zapisuje.</span><span class="sxs-lookup"><span data-stu-id="53578-197">DTUs are based on a blended measure of CPU, memory, reads, and writes.</span></span> <span data-ttu-id="53578-198">Jak zvýšit počet jednotek Dtu, hello napájení, které nabízí zvýšení úrovně výkonu hello.</span><span class="sxs-lookup"><span data-stu-id="53578-198">As DTUs increase, hello power offered by hello performance level increases.</span></span> <span data-ttu-id="53578-199">Například úroveň výkonu s 5 Dtu má pětkrát další výkon než úroveň výkonu s 1 DTU.</span><span class="sxs-lookup"><span data-stu-id="53578-199">For example, a performance level with 5 DTUs has five times more power than a performance level with 1 DTU.</span></span> <span data-ttu-id="53578-200">Maximální kvóty DTU platí tooeach serveru a elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="53578-200">A maximum DTU quota applies tooeach server and elastic pool.</span></span>

<span data-ttu-id="53578-201">Spuštěním hello následující protokolu vyhledávací dotaz lze snadno určit, pokud jsou nedostatečně využívá nebo přes využitím fondech elastické SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="53578-201">By running hello following Log Search query, you can easily tell if you are underutilizing or over utilizing your SQL Azure elastic pools.</span></span>

```
Type=AzureMetrics ResourceId=*"/ELASTICPOOLS/"* MetricName=dtu_consumption_percent | measure avg(Average) by Resource | display LineChart
```

>[!NOTE]
> <span data-ttu-id="53578-202">Pokud pracovní prostor byl upgradovaný toohello [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak hello výše dotazu by změňte následující toohello.</span><span class="sxs-lookup"><span data-stu-id="53578-202">If your workspace has been upgraded toohello [new Log Analytics query language](log-analytics-log-search-upgrade.md), then hello above query would change toohello following.</span></span>
>
>`search in (AzureMetrics) isnotempty(ResourceId) and "/ELASTICPOOLS/" and MetricName == "dtu_consumption_percent" | summarize AggregatedValue = avg(Average) by bin(TimeGenerated, 1h), Resource | render timechart`

<span data-ttu-id="53578-203">V následující ukázka hello, uvidíte, že jeden elastického fondu má vysoké využití téměř 100 % DTU jiné mají velmi malé využití.</span><span class="sxs-lookup"><span data-stu-id="53578-203">In hello following example, you can see that one elastic pool has a high usage near 100% DTU while others have very little usage.</span></span> <span data-ttu-id="53578-204">Můžete prozkoumat další tootroubleshoot potenciální nedávné změny ve vašem prostředí, pomocí protokolů aktivita služby Azure.</span><span class="sxs-lookup"><span data-stu-id="53578-204">You can investigate further tootroubleshoot potential recent changes in your environment using Azure Activity logs.</span></span>

![Výsledky hledání protokolu - vysoké využití](./media/log-analytics-azure-sql/log-search-high-util.png)

## <a name="see-also"></a><span data-ttu-id="53578-206">Viz také</span><span class="sxs-lookup"><span data-stu-id="53578-206">See also</span></span>

- <span data-ttu-id="53578-207">Použití [protokolu hledání](log-analytics-log-searches.md) v tooview analýzy protokolů podrobné dat Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="53578-207">Use [Log Searches](log-analytics-log-searches.md) in Log Analytics tooview detailed Azure SQL data.</span></span>
- <span data-ttu-id="53578-208">[Vytvořit vlastní řídicí panely](log-analytics-dashboards.md) zobrazující dat Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="53578-208">[Create your own dashboards](log-analytics-dashboards.md) showing Azure SQL data.</span></span>
- <span data-ttu-id="53578-209">[Vytvářet výstrahy](log-analytics-alerts.md) při výskytu určité události Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="53578-209">[Create alerts](log-analytics-alerts.md) when specific Azure SQL events occur.</span></span>
