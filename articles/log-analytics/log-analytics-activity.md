---
title: "Zobrazit protokoly Azure aktivitu se analýza protokolů | Microsoft Docs"
description: "Řešení Azure aktivity protokoly můžete použít k analýze a hledání protokol činnosti Azure ve všech vašich předplatných Azure."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: dbac4c73-0058-4191-a906-e59aca8e2ee0
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: banders
ms.openlocfilehash: 1ad56a54f094f3c314596b3a7c9fecd09647d065
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="view-azure-activity-logs"></a><span data-ttu-id="5614e-103">Zobrazit protokoly aktivita Azure</span><span class="sxs-lookup"><span data-stu-id="5614e-103">View Azure activity logs</span></span>

![Azure symbol protokoly aktivity](./media/log-analytics-activity/activity-log-analytics.png)

<span data-ttu-id="5614e-105">Analýzy protokolů aktivity řešení vám pomáhají analyzovat a hledání [protokol činnosti Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) ve všech vašich předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="5614e-105">The Activity Log Analytics solution helps you analyze and search the [Azure activity log](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) across all your Azure subscriptions.</span></span> <span data-ttu-id="5614e-106">Protokol činnosti Azure je protokol, který nabízí přehled operací prováděných s prostředky v rámci vašich předplatných.</span><span class="sxs-lookup"><span data-stu-id="5614e-106">The Azure Activity Log is a log that offers insights into the operations performed on resources in your subscriptions.</span></span> <span data-ttu-id="5614e-107">Protokol aktivit se dřív označovala jako *protokoly auditu* nebo *operační protokoly* vzhledem k tomu, že oznámí události pro vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="5614e-107">The Activity Log was previously known as *Audit Logs* or *Operational Logs* since it reports events for your subscriptions.</span></span>

<span data-ttu-id="5614e-108">Pomocí protokolu činnosti, můžete určit *co*, *kdo*, a *při* pro všechny zápisu operace (PUT, POST, DELETE) pro prostředky ve vašem předplatném.</span><span class="sxs-lookup"><span data-stu-id="5614e-108">Using the Activity Log, you can determine the *what*, *who*, and *when* for any write operations (PUT, POST, DELETE) made for the resources in your subscription.</span></span> <span data-ttu-id="5614e-109">Můžete také chápou stav operace a další relevantní vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="5614e-109">You can also understand the status of the operations and other relevant properties.</span></span> <span data-ttu-id="5614e-110">Protokol aktivit nezahrnuje operace čtení (GET) nebo operace pro prostředky, které používají model nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="5614e-110">The Activity Log does not include read (GET) operations or operations for resources that use the Classic deployment model.</span></span>

<span data-ttu-id="5614e-111">Pokud připojíte k analýze protokolů protokolů aktivita Azure, můžete:</span><span class="sxs-lookup"><span data-stu-id="5614e-111">When you connect your Azure activity logs to Log Analytics, you can:</span></span>

- <span data-ttu-id="5614e-112">Analýza protokolů aktivity s předem definované zobrazení</span><span class="sxs-lookup"><span data-stu-id="5614e-112">Analyze the activity logs with pre-defined views</span></span>
- <span data-ttu-id="5614e-113">Analýza a protokoly vyhledávání a aktivity z několika předplatných Azure</span><span class="sxs-lookup"><span data-stu-id="5614e-113">Analyze and search and activity logs from multiple Azure subscriptions</span></span>
- <span data-ttu-id="5614e-114">Zachovat protokoly aktivity po dobu delší než 90 dní<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="5614e-114">Keep activity logs for longer than 90 days<sup>1</sup></span></span>
- <span data-ttu-id="5614e-115">Korelovat protokoly aktivity s další Azure data platformy a aplikace</span><span class="sxs-lookup"><span data-stu-id="5614e-115">Correlate activity logs with other Azure platform and application data</span></span>
- <span data-ttu-id="5614e-116">Najdete v provozní aktivity agregovat podle stavu</span><span class="sxs-lookup"><span data-stu-id="5614e-116">See operational activities aggregated by status</span></span>
- <span data-ttu-id="5614e-117">Zobrazení trendů aktivit děje na všech služeb Azure</span><span class="sxs-lookup"><span data-stu-id="5614e-117">View trends of activities happening on each of your Azure services</span></span>
- <span data-ttu-id="5614e-118">Zprávu o autorizaci změny na všech vašich prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="5614e-118">Report on authorization changes on all your Azure resources</span></span>
- <span data-ttu-id="5614e-119">Určit vaše prostředky, které mají vliv výpadku nebo služby týkající se stavu</span><span class="sxs-lookup"><span data-stu-id="5614e-119">Identify outage or service health issues impacting your resources</span></span>
- <span data-ttu-id="5614e-120">Použijte funkci vyhledávání protokolu ke korelaci aktivity uživatelů, operace automatického škálování, změny autorizace a stav služby na jiné protokoly nebo metriky ze svého prostředí</span><span class="sxs-lookup"><span data-stu-id="5614e-120">Use Log Search to correlate user activities, auto-scale operations, authorization changes, and service health to other logs or metrics from your environment</span></span>

<span data-ttu-id="5614e-121"><sup>1</sup>ve výchozím nastavení, analýzy protokolů udržuje protokolů Azure aktivity dobu 90 dnů, i když jsou na úrovni Free.</span><span class="sxs-lookup"><span data-stu-id="5614e-121"><sup>1</sup>By default, Log Analytics keeps your Azure activity logs for 90 days, even if you are on the Free tier.</span></span> <span data-ttu-id="5614e-122">Nebo, pokud máte pracovní prostor nastavení uchování menší než 90 dní.</span><span class="sxs-lookup"><span data-stu-id="5614e-122">Or, if you have a workspace retention setting of less than 90 days.</span></span> <span data-ttu-id="5614e-123">Pokud pracovní prostor uchovávání dat, který je delší než 90 dní, protokoly aktivity budou pro dobu uchování vašeho pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="5614e-123">If your workspace has retention that is longer than 90 days, the activity logs are kept for the retention period of your workspace.</span></span>

<span data-ttu-id="5614e-124">Analýzy protokolů shromažďuje protokoly aktivity, které jsou zdarma a ukládá protokoly 90 dní zdarma.</span><span class="sxs-lookup"><span data-stu-id="5614e-124">Log Analytics collects activity logs free of charge and stores the logs for 90 days free of charge.</span></span> <span data-ttu-id="5614e-125">Pokud ukládáte protokoly víc než 90 dnů, bude platit poplatky uchovávání dat pro data uložená déle než 90 dní.</span><span class="sxs-lookup"><span data-stu-id="5614e-125">If you store logs for longer than 90 days, you will incur data retention charges for the data stored longer than 90 days.</span></span>

<span data-ttu-id="5614e-126">Pokud jste na cenové úrovně Free, protokoly aktivity se nevztahují na vaše denní spotřeba data.</span><span class="sxs-lookup"><span data-stu-id="5614e-126">When you're on the Free pricing tier, activity logs do not apply to your daily data consumption.</span></span>

## <a name="connected-sources"></a><span data-ttu-id="5614e-127">Připojené zdroje</span><span class="sxs-lookup"><span data-stu-id="5614e-127">Connected sources</span></span>

<span data-ttu-id="5614e-128">Na rozdíl od většiny jiných řešení analýzy protokolů není data shromážděná protokoly aktivity pomocí agentů.</span><span class="sxs-lookup"><span data-stu-id="5614e-128">Unlike most other Log Analytics solutions, data isn't collected for activity logs by agents.</span></span> <span data-ttu-id="5614e-129">Všechna data, která používá řešení pochází přímo z Azure.</span><span class="sxs-lookup"><span data-stu-id="5614e-129">All data used by the solution comes directly from Azure.</span></span>

| <span data-ttu-id="5614e-130">Připojený zdroj</span><span class="sxs-lookup"><span data-stu-id="5614e-130">Connected Source</span></span> | <span data-ttu-id="5614e-131">Podporuje se</span><span class="sxs-lookup"><span data-stu-id="5614e-131">Supported</span></span> | <span data-ttu-id="5614e-132">Popis</span><span class="sxs-lookup"><span data-stu-id="5614e-132">Description</span></span> |
| --- | --- | --- |
| [<span data-ttu-id="5614e-133">Agenti systému Windows</span><span class="sxs-lookup"><span data-stu-id="5614e-133">Windows agents</span></span>](log-analytics-windows-agents.md) | <span data-ttu-id="5614e-134">Ne</span><span class="sxs-lookup"><span data-stu-id="5614e-134">No</span></span> | <span data-ttu-id="5614e-135">Řešení neshromažďuje informace z agentů v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="5614e-135">The solution does not collect information from Windows agents.</span></span> |
| [<span data-ttu-id="5614e-136">Agenti systému Linux</span><span class="sxs-lookup"><span data-stu-id="5614e-136">Linux agents</span></span>](log-analytics-linux-agents.md) | <span data-ttu-id="5614e-137">Ne</span><span class="sxs-lookup"><span data-stu-id="5614e-137">No</span></span> | <span data-ttu-id="5614e-138">Řešení neshromažďuje informace od agentů systému Linux.</span><span class="sxs-lookup"><span data-stu-id="5614e-138">The solution does not collect information from Linux agents.</span></span> |
| [<span data-ttu-id="5614e-139">Skupiny správy nástroje SCOM</span><span class="sxs-lookup"><span data-stu-id="5614e-139">SCOM management group</span></span>](log-analytics-om-agents.md) | <span data-ttu-id="5614e-140">Ne</span><span class="sxs-lookup"><span data-stu-id="5614e-140">No</span></span> | <span data-ttu-id="5614e-141">Řešení neshromažďuje informace od agentů v připojené skupině pro správu SCOM.</span><span class="sxs-lookup"><span data-stu-id="5614e-141">The solution does not collect information from agents in a connected SCOM management group.</span></span> |
| [<span data-ttu-id="5614e-142">Účet služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="5614e-142">Azure storage account</span></span>](log-analytics-azure-storage.md) | <span data-ttu-id="5614e-143">Ne</span><span class="sxs-lookup"><span data-stu-id="5614e-143">No</span></span> | <span data-ttu-id="5614e-144">Řešení neshromažďuje informace ze služby Azure storage.</span><span class="sxs-lookup"><span data-stu-id="5614e-144">The solution does not collect information from Azure storage.</span></span> |

## <a name="prerequisites"></a><span data-ttu-id="5614e-145">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5614e-145">Prerequisites</span></span>

- <span data-ttu-id="5614e-146">Pro přístup k informacím protokolu aktivita Azure, musíte mít předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="5614e-146">To access Azure activity log information, you must have an Azure subscription.</span></span>

## <a name="configuration"></a><span data-ttu-id="5614e-147">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="5614e-147">Configuration</span></span>

<span data-ttu-id="5614e-148">Proveďte následující postup pro konfiguraci řešení aktivity analýzy protokolů pro pracovní prostory.</span><span class="sxs-lookup"><span data-stu-id="5614e-148">Perform the following steps to configure the Activity Log Analytics solution for your workspaces.</span></span>

1. <span data-ttu-id="5614e-149">Povolit řešení analýzy protokolů aktivity z [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureActivityOMS?tab=Overview) nebo pomocí procesu popsaného v tématu [řešení přidat analýzy protokolů z Galerie řešení](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="5614e-149">Enable the Activity Log Analytics solution from the [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureActivityOMS?tab=Overview) or by using the process described in [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md).</span></span>
2. <span data-ttu-id="5614e-150">Konfigurace protokolů událostí přejděte do pracovního prostoru analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="5614e-150">Configure activity logs to go to your Log Analytics workspace.</span></span>
    1. <span data-ttu-id="5614e-151">Na portálu Azure vyberte pracovní prostor a pak klikněte na tlačítko **protokol činnosti Azure**.</span><span class="sxs-lookup"><span data-stu-id="5614e-151">In the Azure portal, select your workspace and then click **Azure Activity log**.</span></span>
    2. <span data-ttu-id="5614e-152">Pro každé předplatné klikněte na název odběru.</span><span class="sxs-lookup"><span data-stu-id="5614e-152">For each subscription, click the subscription name.</span></span>  
        ![Přidat předplatné](./media/log-analytics-activity/add-subscription.png)
    3. <span data-ttu-id="5614e-154">V *Název_předplatného* okně klikněte na tlačítko **Connect**.</span><span class="sxs-lookup"><span data-stu-id="5614e-154">In the *SubscriptionName* blade, click **Connect**.</span></span>  
        <span data-ttu-id="5614e-155">![připojení odběru](./media/log-analytics-activity/subscription-connect.png)</span><span class="sxs-lookup"><span data-stu-id="5614e-155">![connect subscription](./media/log-analytics-activity/subscription-connect.png)</span></span>

<span data-ttu-id="5614e-156">Pokud přidáte řešení pomocí portálu OMS, uvidíte následující dlaždice.</span><span class="sxs-lookup"><span data-stu-id="5614e-156">If you add the solution using the OMS portal, you'll see the following tile.</span></span> <span data-ttu-id="5614e-157">Přihlásit se k portálu Azure a předplatné Azure připojit do pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="5614e-157">Sign in to the Azure portal to connect an Azure subscription to your workspace.</span></span>  
![provádění hodnocení](./media/log-analytics-activity/tile-performing-assessment.png)

## <a name="using-the-solution"></a><span data-ttu-id="5614e-159">Použití řešení</span><span class="sxs-lookup"><span data-stu-id="5614e-159">Using the solution</span></span>

<span data-ttu-id="5614e-160">Když přidáte řešení analýzy protokolů aktivitu do pracovního prostoru **protokoly aktivity Azure** dlaždice se přidá na řídicí panel Přehled.</span><span class="sxs-lookup"><span data-stu-id="5614e-160">When you add the Activity Log Analytics solution to your workspace, the **Azure Activity Logs** tile is added to your Overview dashboard.</span></span> <span data-ttu-id="5614e-161">Tuto dlaždici zobrazí počet záznamů aktivita Azure pro předplatná Azure, která má přístup k řešení.</span><span class="sxs-lookup"><span data-stu-id="5614e-161">This tile displays a count of the number of Azure activity records for the Azure subscriptions that the solution has access to.</span></span>

![Azure dlaždice protokoly aktivity](./media/log-analytics-activity/azure-activity-logs-tile.png)

### <a name="view-azure-activity-logs"></a><span data-ttu-id="5614e-163">Aktivita služby Azure zobrazení protokolů</span><span class="sxs-lookup"><span data-stu-id="5614e-163">View Azure Activity logs</span></span>

<span data-ttu-id="5614e-164">Klikněte na tlačítko **protokoly aktivity Azure** dlaždici otevřete **protokoly aktivity Azure** řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="5614e-164">Click the **Azure Activity Logs** tile to open the **Azure Activity Logs** dashboard.</span></span> <span data-ttu-id="5614e-165">Řídicí panel obsahuje okna v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="5614e-165">The dashboard includes the blades in the following table.</span></span> <span data-ttu-id="5614e-166">Každý okno uvádí až 10 položky odpovídající kritériím tohoto okna pro zadaný obor a časový rozsah.</span><span class="sxs-lookup"><span data-stu-id="5614e-166">Each blade lists up to 10 items matching that blade's criteria for the specified scope and time range.</span></span> <span data-ttu-id="5614e-167">Můžete spustit vyhledávání protokolu, který vrátí všechny záznamy kliknutím **zobrazit všechny** v dolní části okna, nebo kliknutím na záhlaví okna.</span><span class="sxs-lookup"><span data-stu-id="5614e-167">You can run a log search that returns all records by clicking **See all** at the bottom of the blade or by clicking the blade header.</span></span>

<span data-ttu-id="5614e-168">Data protokolu aktivity se zobrazí pouze *po* protokolů aktivity k přejít na řešení, takže data nelze zobrazit, předtím jste nakonfigurovali.</span><span class="sxs-lookup"><span data-stu-id="5614e-168">Activity log data only appears *after* you've configured your activity logs to go to the solution, so you can't view data before then.</span></span>

| <span data-ttu-id="5614e-169">Okno</span><span class="sxs-lookup"><span data-stu-id="5614e-169">Blade</span></span> | <span data-ttu-id="5614e-170">Popis</span><span class="sxs-lookup"><span data-stu-id="5614e-170">Description</span></span> |
| --- | --- |
| <span data-ttu-id="5614e-171">Aktivita Azure položky protokolu</span><span class="sxs-lookup"><span data-stu-id="5614e-171">Azure Activity Log Entries</span></span> | <span data-ttu-id="5614e-172">Ukazuje pruhový graf prvních položka protokolu aktivita Azure záznamů součty pro rozsah dat, které jste vybrali a seznam top 10 aktivity volající.</span><span class="sxs-lookup"><span data-stu-id="5614e-172">Shows a bar chart of the top Azure activity log entry record totals for the date range that you have selected and shows a list of the top 10 activity callers.</span></span> <span data-ttu-id="5614e-173">Klikněte na tlačítko pruhový graf ke spuštění protokolu vyhledejte <code>Type=AzureActivity</code>.</span><span class="sxs-lookup"><span data-stu-id="5614e-173">Click the bar chart to run a log search for <code>Type=AzureActivity</code>.</span></span> <span data-ttu-id="5614e-174">Klikněte na položku volající ke spuštění vyhledávání protokolu vrácení všech položek protokolů aktivity pro tuto položku.</span><span class="sxs-lookup"><span data-stu-id="5614e-174">Click a caller item to run a log search returning all activity log entries for that item.</span></span> |
| <span data-ttu-id="5614e-175">Protokoly aktivity podle stavu</span><span class="sxs-lookup"><span data-stu-id="5614e-175">Activity Logs by Status</span></span> | <span data-ttu-id="5614e-176">Zobrazí prstencový graf pro stav protokolu Azure aktivity pro rozsah, kterou jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="5614e-176">Shows a doughnut chart for Azure activity log status for the date range that you have selected.</span></span> <span data-ttu-id="5614e-177">Také ukazuje seznam seznam top deset stav záznamů.</span><span class="sxs-lookup"><span data-stu-id="5614e-177">Also shows a list a list of the top ten status records.</span></span> <span data-ttu-id="5614e-178">Klikněte na graf ke spuštění protokolu vyhledejte <code>Type=AzureActivity &#124; measure count() by ActivityStatus</code>.</span><span class="sxs-lookup"><span data-stu-id="5614e-178">Click the chart to run a log search for <code>Type=AzureActivity &#124; measure count() by ActivityStatus</code>.</span></span> <span data-ttu-id="5614e-179">Klikněte na položku Stav spuštění vyhledávání protokolu vrácení všech položek protokolů aktivity pro tento stav záznamu.</span><span class="sxs-lookup"><span data-stu-id="5614e-179">Click a status item to run a log search returning all activity log entries for that status record.</span></span> |
| <span data-ttu-id="5614e-180">Protokoly aktivity podle zdroje</span><span class="sxs-lookup"><span data-stu-id="5614e-180">Activity Logs by Resource</span></span> | <span data-ttu-id="5614e-181">Zobrazuje celkový počet prostředků s protokoly aktivity a uvádí top deset prostředky pomocí záznamu počty každého prostředku.</span><span class="sxs-lookup"><span data-stu-id="5614e-181">Shows the total number of resources with activity logs and lists the top ten resources with record counts for each resource.</span></span> <span data-ttu-id="5614e-182">Klikněte na tlačítko Spustit hledání protokolů pro oblasti celkový <code>Type=AzureActivity &#124; measure count() by Resource</code>, který zobrazuje všechny prostředky Azure k dispozici pro řešení.</span><span class="sxs-lookup"><span data-stu-id="5614e-182">Click the total area to run a log search for <code>Type=AzureActivity &#124; measure count() by Resource</code>, which shows all Azure resources available to the solution.</span></span> <span data-ttu-id="5614e-183">Klikněte na tlačítko prostředků ke spuštění vyhledávání protokolu vrátit všechny záznamy aktivity pro tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="5614e-183">Click a resource to run a log search returning all activity records for that resource.</span></span> |
| <span data-ttu-id="5614e-184">Protokoly aktivity poskytovatelem prostředků</span><span class="sxs-lookup"><span data-stu-id="5614e-184">Activity Logs by Resource Provider</span></span> | <span data-ttu-id="5614e-185">Zobrazuje celkový počet poskytovatelů prostředků, které produkují aktivity protokoly a uvádí deset.</span><span class="sxs-lookup"><span data-stu-id="5614e-185">Shows the total number of resource providers that produce activity logs and lists the top ten.</span></span> <span data-ttu-id="5614e-186">Klikněte na tlačítko Spustit hledání protokolů pro oblasti celkový <code>Type=AzureActivity &#124; measure count() by ResourceProvider</code>, který zobrazuje všechny poskytovatele prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="5614e-186">Click the total area to run a log search for <code>Type=AzureActivity &#124; measure count() by ResourceProvider</code>, which shows all Azure resource providers.</span></span> <span data-ttu-id="5614e-187">Klikněte na zprostředkovatele prostředků ke spuštění vyhledávání protokolu vrátit všechny záznamy aktivity pro zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="5614e-187">Click a resource provider to run a log search returning all activity records for the provider.</span></span> |

![Řídicí panel Azure protokoly aktivity](./media/log-analytics-activity/activity-log-dash.png)

## <a name="next-steps"></a><span data-ttu-id="5614e-189">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5614e-189">Next steps</span></span>

- <span data-ttu-id="5614e-190">Vytvoření [výstraha](log-analytics-alerts-creating.md) když se stane konkrétní aktivity.</span><span class="sxs-lookup"><span data-stu-id="5614e-190">Create an [alert](log-analytics-alerts-creating.md) when a specific activity happens.</span></span>
- <span data-ttu-id="5614e-191">Použití [hledání protokolů](log-analytics-log-searches.md) k zobrazení podrobných informací z protokolů aktivity.</span><span class="sxs-lookup"><span data-stu-id="5614e-191">Use [Log Search](log-analytics-log-searches.md) to view detailed information from your activity logs.</span></span>