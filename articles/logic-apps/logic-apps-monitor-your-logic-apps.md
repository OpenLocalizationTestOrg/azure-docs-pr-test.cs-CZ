---
title: "Zkontrolujte stav, nastavení protokolování a získat upozornění – Azure Logic Apps | Microsoft Docs"
description: "Monitorovat stav a výkon pro aplikace logiky, protokolu diagnostická data a nastavení výstrah"
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 5c1b1e15-3b6c-49dc-98a6-bdbe7cb75339
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 07/21/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 4795f5728d4ce6ff21b97bc3fefd6a53e0c6a11b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="monitor-status-set-up-diagnostics-logging-and-turn-on-alerts-for-azure-logic-apps"></a><span data-ttu-id="d1145-103">Monitorování stavu, nastavit protokolování diagnostiky a zapnutí výstrah pro Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="d1145-103">Monitor status, set up diagnostics logging, and turn on alerts for Azure Logic Apps</span></span>

<span data-ttu-id="d1145-104">Po jste [vytvořte a spusťte aplikaci logiky](../logic-apps/logic-apps-create-a-logic-app.md), můžete zkontrolovat historii spustí, historie aktivační událost, stavu a výkonu.</span><span class="sxs-lookup"><span data-stu-id="d1145-104">After you [create and run a logic app](../logic-apps/logic-apps-create-a-logic-app.md), you can check its runs history, trigger history, status, and performance.</span></span> <span data-ttu-id="d1145-105">Nastavení pro sledování událostí v reálném čase a bohatší ladění, [protokolování diagnostiky](#azure-diagnostics) pro svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="d1145-105">For real-time event monitoring and richer debugging, set up [diagnostics logging](#azure-diagnostics) for your logic app.</span></span> <span data-ttu-id="d1145-106">Tímto způsobem můžete [najít a zobrazit události](#find-events), jako jsou aktivační události, spuštění události a události akce.</span><span class="sxs-lookup"><span data-stu-id="d1145-106">That way, you can [find and view events](#find-events), like trigger events, run events, and action events.</span></span> <span data-ttu-id="d1145-107">Můžete také použít tuto [diagnostická data s jinými službami](#extend-diagnostic-data), jako jsou služby Azure Storage a Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="d1145-107">You can also use this [diagnostics data with other services](#extend-diagnostic-data), like Azure Storage and Azure Event Hubs.</span></span> 

<span data-ttu-id="d1145-108">Pokud chcete dostávat oznámení o selhání nebo jiné možné problémy, nastavte [výstrahy](#add-azure-alerts).</span><span class="sxs-lookup"><span data-stu-id="d1145-108">To get notifications about failures or other possible problems, set up [alerts](#add-azure-alerts).</span></span> <span data-ttu-id="d1145-109">Například můžete vytvořit výstrahu, která zjistí "při více než pět spustí nezdaří za hodinu."</span><span class="sxs-lookup"><span data-stu-id="d1145-109">For example, you can create an alert that detects "when more than five runs fail in an hour."</span></span> <span data-ttu-id="d1145-110">Můžete také nastavit monitorování, sledování a protokolování programově pomocí [Azure Diagnostics událostí nastavení a vlastností](#diagnostic-event-properties).</span><span class="sxs-lookup"><span data-stu-id="d1145-110">You can also set up monitoring, tracking, and logging programmatically by using [Azure Diagnostics event settings and properties](#diagnostic-event-properties).</span></span>

## <a name="view-runs-and-trigger-history-for-your-logic-app"></a><span data-ttu-id="d1145-111">Spustí zobrazení a historii aktivační událost pro svou aplikaci logiky</span><span class="sxs-lookup"><span data-stu-id="d1145-111">View runs and trigger history for your logic app</span></span>

1. <span data-ttu-id="d1145-112">K vyhledání aplikace logiky v [portál Azure](https://portal.azure.com), v hlavní nabídce Azure, zvolte **další služby**.</span><span class="sxs-lookup"><span data-stu-id="d1145-112">To find your logic app in the [Azure portal](https://portal.azure.com), on the main Azure menu, choose **More services**.</span></span> <span data-ttu-id="d1145-113">Do vyhledávacího pole, vyhledejte "aplikace logiky" a zvolte **Logic apps**.</span><span class="sxs-lookup"><span data-stu-id="d1145-113">In the search box, find "logic apps", and choose **Logic apps**.</span></span>

   ![Najít aplikaci logiky](./media/logic-apps-monitor-your-logic-apps/find-your-logic-app.png)

   <span data-ttu-id="d1145-115">Portálu Azure obsahuje všechny aplikace logiky, které jsou spojeny s předplatným Azure.</span><span class="sxs-lookup"><span data-stu-id="d1145-115">The Azure portal shows all the logic apps that are associated with your Azure subscription.</span></span> 

2. <span data-ttu-id="d1145-116">Vyberte svou aplikaci logiky, a potom vyberte **přehled**.</span><span class="sxs-lookup"><span data-stu-id="d1145-116">Select your logic app, then choose **Overview**.</span></span>

   <span data-ttu-id="d1145-117">Portál Azure se zobrazuje historie spustí a historii aktivační událost pro svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="d1145-117">The Azure portal shows the runs history and trigger history for your logic app.</span></span> <span data-ttu-id="d1145-118">Například:</span><span class="sxs-lookup"><span data-stu-id="d1145-118">For example:</span></span>

   ![Spuštění aplikace logiky historie historie a aktivační události](media/logic-apps-monitor-your-logic-apps/overview.png)

   * <span data-ttu-id="d1145-120">**Spustí historie** ukazuje všechny spuštění pro svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="d1145-120">**Runs history** shows all the runs for your logic app.</span></span> 
   * <span data-ttu-id="d1145-121">**Aktivovat historie** zobrazuje všechny aktivity aktivační událost pro svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="d1145-121">**Trigger History** shows all the trigger activity for your logic app.</span></span>

   <span data-ttu-id="d1145-122">Popis stavu najdete v tématu [řešení aplikace logiky](../logic-apps/logic-apps-diagnosing-failures.md).</span><span class="sxs-lookup"><span data-stu-id="d1145-122">For status descriptions, see [Troubleshoot your logic app](../logic-apps/logic-apps-diagnosing-failures.md).</span></span>

   > [!TIP]
   > <span data-ttu-id="d1145-123">Pokud nenajdete data, která očekáváte, na panelu nástrojů vyberte **aktualizovat**.</span><span class="sxs-lookup"><span data-stu-id="d1145-123">If you don't find the data that you expect, on the toolbar, choose **Refresh**.</span></span>

3. <span data-ttu-id="d1145-124">Chcete-li zobrazit kroky z konkrétní spuštění, v části **spouští historie**, vyberte, které používají.</span><span class="sxs-lookup"><span data-stu-id="d1145-124">To view the steps from a specific run, under **Runs history**, select that run.</span></span> 

   <span data-ttu-id="d1145-125">Zobrazení monitorování uvádí každý krok v tom, že spustit.</span><span class="sxs-lookup"><span data-stu-id="d1145-125">The monitor view shows each step in that run.</span></span> <span data-ttu-id="d1145-126">Například:</span><span class="sxs-lookup"><span data-stu-id="d1145-126">For example:</span></span>

   ![Akce pro konkrétní spustit](media/logic-apps-monitor-your-logic-apps/monitor-view-updated.png)

4. <span data-ttu-id="d1145-128">Chcete-li získat další informace o spuštění, zvolte **podrobnosti o spuštění**.</span><span class="sxs-lookup"><span data-stu-id="d1145-128">To get more details about the run, choose **Run Details**.</span></span> <span data-ttu-id="d1145-129">Tyto informace shrnuje kroky, stav, vstupy a výstupy pro spuštění.</span><span class="sxs-lookup"><span data-stu-id="d1145-129">This information summarizes the steps, status, inputs, and outputs for the run.</span></span> 

   ![Zvolte "Spustit podrobnosti"](media/logic-apps-monitor-your-logic-apps/run-details.png)

   <span data-ttu-id="d1145-131">Například můžete získat spuštění **ID korelace**, což může být nutné při použití [REST API pro Logic Apps](https://docs.microsoft.com/rest/api/logic).</span><span class="sxs-lookup"><span data-stu-id="d1145-131">For example, you can get the run's **Correlation ID**, which you might need when you use the [REST API for Logic Apps](https://docs.microsoft.com/rest/api/logic).</span></span>

5. <span data-ttu-id="d1145-132">Chcete-li získat podrobnosti o konkrétní krok, zvolte tohoto kroku.</span><span class="sxs-lookup"><span data-stu-id="d1145-132">To get details about a specific step, choose that step.</span></span> <span data-ttu-id="d1145-133">Nyní můžete zkontrolovat podrobnosti, třeba vstupy, výstupy a všechny chyby, které pro tento krok se stalo.</span><span class="sxs-lookup"><span data-stu-id="d1145-133">You can now review details like inputs, outputs, and any errors that happened for that step.</span></span> <span data-ttu-id="d1145-134">Například:</span><span class="sxs-lookup"><span data-stu-id="d1145-134">For example:</span></span>

   ![Podrobnosti o krok](media/logic-apps-monitor-your-logic-apps/monitor-view-details.png)
   
   > [!NOTE]
   > <span data-ttu-id="d1145-136">Všechny podrobnosti o modulu runtime a události se šifrují v rámci služby Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="d1145-136">All runtime details and events are encrypted within the Logic Apps service.</span></span> <span data-ttu-id="d1145-137">Budou se dešifrují jenom v případě, že uživatel požádá o zobrazovat data.</span><span class="sxs-lookup"><span data-stu-id="d1145-137">They are decrypted only when a user requests to view that data.</span></span> <span data-ttu-id="d1145-138">Také můžete řídit přístup k těmto událostem s [řízení řízení přístupu (RBAC)](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="d1145-138">You can also control access to these events with [Azure Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-what-is.md).</span></span>

6. <span data-ttu-id="d1145-139">Chcete-li získat podrobnosti o konkrétní aktivační událost, přejděte zpět na **přehled** podokně.</span><span class="sxs-lookup"><span data-stu-id="d1145-139">To get details about a specific trigger event, go back to the **Overview** pane.</span></span> <span data-ttu-id="d1145-140">V části **aktivovat historie**, vyberte aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="d1145-140">Under **Trigger history**, select the trigger event.</span></span> <span data-ttu-id="d1145-141">Můžete teď zkontrolujte podrobnosti jako vstupy a výstupy, například:</span><span class="sxs-lookup"><span data-stu-id="d1145-141">You can now review details like inputs and outputs, for example:</span></span>

   ![Podrobnosti o aktivační události výstup](media/logic-apps-monitor-your-logic-apps/trigger-details.png)

<a name="azure-diagnostics"></a>

## <a name="turn-on-diagnostics-logging-for-your-logic-app"></a><span data-ttu-id="d1145-143">Zapněte diagnostiku protokolování pro svou aplikaci logiky</span><span class="sxs-lookup"><span data-stu-id="d1145-143">Turn on diagnostics logging for your logic app</span></span>

<span data-ttu-id="d1145-144">Pro širší ladění s podrobnostmi runtime a události, můžete nastavit protokolování s diagnostiky [Azure Log Analytics](../log-analytics/log-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d1145-144">For richer debugging with runtime details and events, you can set up diagnostics logging with [Azure Log Analytics](../log-analytics/log-analytics-overview.md).</span></span> <span data-ttu-id="d1145-145">Analýzy protokolů je služba v [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) , sleduje vaše cloudové a místní prostředí pro správu jejich dostupnost a výkon.</span><span class="sxs-lookup"><span data-stu-id="d1145-145">Log Analytics is a service in [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) that monitors your cloud and on-premises environments to help you maintain their availability and performance.</span></span> 

<span data-ttu-id="d1145-146">Než začnete, musíte mít pracovním prostorem OMS.</span><span class="sxs-lookup"><span data-stu-id="d1145-146">Before you start, you need to have an OMS workspace.</span></span> <span data-ttu-id="d1145-147">Další informace [jak vytvořit pracovní prostor služby OMS](../log-analytics/log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="d1145-147">Learn [how to create an OMS workspace](../log-analytics/log-analytics-get-started.md).</span></span>

1. <span data-ttu-id="d1145-148">V [portál Azure](https://portal.azure.com), najděte a vyberte svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="d1145-148">In the [Azure portal](https://portal.azure.com), find and select your logic app.</span></span> 

2. <span data-ttu-id="d1145-149">V nabídce okno aplikace logiky v rámci **monitorování**, zvolte **diagnostiky** > **nastavení pro diagnostiku**.</span><span class="sxs-lookup"><span data-stu-id="d1145-149">On the logic app blade menu, under **Monitoring**, choose **Diagnostics** > **Diagnostic Settings**.</span></span>

   ![Přejděte na monitorování, Diagnostika, nastavení pro diagnostiku](media/logic-apps-monitor-your-logic-apps/logic-app-diagnostics.png)

3. <span data-ttu-id="d1145-151">V části **nastavení diagnostiky**, zvolte **na**.</span><span class="sxs-lookup"><span data-stu-id="d1145-151">Under **Diagnostics settings**, choose **On**.</span></span>

   ![Zapnout diagnostické protokoly](media/logic-apps-monitor-your-logic-apps/turn-on-diagnostics-logic-app.png)

4. <span data-ttu-id="d1145-153">Nyní vyberte OMS pracovní prostor a událostí kategorie pro protokolování, jak je znázorněno:</span><span class="sxs-lookup"><span data-stu-id="d1145-153">Now select the OMS workspace and event category for logging as shown:</span></span>

   1. <span data-ttu-id="d1145-154">Vyberte **odeslat k analýze protokolů**.</span><span class="sxs-lookup"><span data-stu-id="d1145-154">Select **Send to Log Analytics**.</span></span> 
   2. <span data-ttu-id="d1145-155">V části **analýzy protokolů**, zvolte **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="d1145-155">Under **Log Analytics**, choose **Configure**.</span></span> 
   3. <span data-ttu-id="d1145-156">V části **pracovních prostorů OMS**, vyberte pracovní prostor OMS pro protokolování.</span><span class="sxs-lookup"><span data-stu-id="d1145-156">Under **OMS Workspaces**, select the OMS workspace to use for logging.</span></span>
   4. <span data-ttu-id="d1145-157">V části **protokolu**, vyberte **modul runtime pracovního postupu** kategorie.</span><span class="sxs-lookup"><span data-stu-id="d1145-157">Under **Log**, select the **WorkflowRuntime** category.</span></span>
   5. <span data-ttu-id="d1145-158">Zvolte metriky interval.</span><span class="sxs-lookup"><span data-stu-id="d1145-158">Choose the metric interval.</span></span>
   6. <span data-ttu-id="d1145-159">Až budete hotoví, zvolte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d1145-159">When you're done, choose **Save**.</span></span>

   ![Vyberte pracovní prostor OMS a dat pro protokolování](media/logic-apps-monitor-your-logic-apps/send-diagnostics-data-log-analytics-workspace.png)

<span data-ttu-id="d1145-161">Teď můžete najít události a dalších dat pro aktivační události, spustit události a události akce.</span><span class="sxs-lookup"><span data-stu-id="d1145-161">Now, you can find events and other data for trigger events, run events, and action events.</span></span>

<a name="find-events"></a>

## <a name="find-events-and-data-for-your-logic-app"></a><span data-ttu-id="d1145-162">Najít události a data pro svou aplikaci logiky</span><span class="sxs-lookup"><span data-stu-id="d1145-162">Find events and data for your logic app</span></span>

<span data-ttu-id="d1145-163">Vyhledat a zobrazit události ve vaší aplikaci logiky, jako například aktivovat události, spustit události a akci události, postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="d1145-163">To find and view events in your logic app, like trigger events, run events, and action events, follow these steps.</span></span>

1. <span data-ttu-id="d1145-164">V [portál Azure](https://portal.azure.com), zvolte **více služeb**.</span><span class="sxs-lookup"><span data-stu-id="d1145-164">In the [Azure portal](https://portal.azure.com), choose **More Services**.</span></span> <span data-ttu-id="d1145-165">Vyhledejte "analýzy protokolů" a pak zvolte **analýzy protokolů** jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="d1145-165">Search for "log analytics", then choose **Log Analytics** as shown here:</span></span>

   ![Zvolte "Analýzy protokolů"](media/logic-apps-monitor-your-logic-apps/browseloganalytics.png)

2. <span data-ttu-id="d1145-167">V části **analýzy protokolů**, najděte a vyberte pracovní prostor OMS.</span><span class="sxs-lookup"><span data-stu-id="d1145-167">Under **Log Analytics**, find and select your OMS workspace.</span></span> 

   ![Vyberte pracovní prostor OMS](media/logic-apps-monitor-your-logic-apps/selectla.png)

3. <span data-ttu-id="d1145-169">V části **správy**, zvolte **portálu OMS**.</span><span class="sxs-lookup"><span data-stu-id="d1145-169">Under **Management**, choose **OMS Portal**.</span></span>

   ![Zvolte "Portál OMS"](media/logic-apps-monitor-your-logic-apps/omsportalpage.png)

4. <span data-ttu-id="d1145-171">Na domovské stránce OMS, zvolte **hledání protokolů**.</span><span class="sxs-lookup"><span data-stu-id="d1145-171">On your OMS home page, choose **Log Search**.</span></span>

   ![Na domovské stránce OMS zvolte "Hledání protokolů"](media/logic-apps-monitor-your-logic-apps/logsearch.png)

   <span data-ttu-id="d1145-173">-nebo-</span><span class="sxs-lookup"><span data-stu-id="d1145-173">-or-</span></span>

   ![V nabídce OMS zvolte "Protokolu hledání"](media/logic-apps-monitor-your-logic-apps/logsearch-2.png)

5. <span data-ttu-id="d1145-175">Do vyhledávacího pole zadejte pole, které chcete najít a stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="d1145-175">In the search box, specify a field that you want to find, and press **Enter**.</span></span> <span data-ttu-id="d1145-176">Když začnete psát, ukazuje OMS je možné shody a operací, které můžete použít.</span><span class="sxs-lookup"><span data-stu-id="d1145-176">When you start typing, OMS shows you possible matches and operations that you can use.</span></span> 

   <span data-ttu-id="d1145-177">Například pokud chcete najít prvních 10 události, které se stalo, zadejte a vyberte tento vyhledávací dotaz: **kategorie = modul runtime pracovního postupu | top 10**</span><span class="sxs-lookup"><span data-stu-id="d1145-177">For example, to find the top 10 events that happened, enter and select this search query: **Category=WorkflowRuntime |top 10**</span></span>

   ![Zadejte hledaný řetězec](media/logic-apps-monitor-your-logic-apps/oms-start-query.png)

   <span data-ttu-id="d1145-179">Další informace o [vyhledávání dat v analýzy protokolů](../log-analytics/log-analytics-log-searches.md).</span><span class="sxs-lookup"><span data-stu-id="d1145-179">Learn more about [how to find data in Log Analytics](../log-analytics/log-analytics-log-searches.md).</span></span>

6. <span data-ttu-id="d1145-180">Na stránce výsledky v levém panelu, vyberte časový rámec, který chcete zobrazit.</span><span class="sxs-lookup"><span data-stu-id="d1145-180">On the results page, in the left bar, choose the timeframe that you want to view.</span></span>
<span data-ttu-id="d1145-181">Chcete-li upřesněte dotaz přidáním filtru, zvolte **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="d1145-181">To refine your query by adding a filter, choose **+Add**.</span></span>

   ![Vyberte časový rámec pro výsledky dotazu](media/logic-apps-monitor-your-logic-apps/query-results.png)

7. <span data-ttu-id="d1145-183">V části **přidat filtry**, zadejte název filtru filtr, který chcete vyhledávat.</span><span class="sxs-lookup"><span data-stu-id="d1145-183">Under **Add Filters**, enter the filter name so you can find the filter you want.</span></span> <span data-ttu-id="d1145-184">Vyberte filtr a vyberte **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="d1145-184">Select the filter, and choose **+Add**.</span></span>

   <span data-ttu-id="d1145-185">Tento příklad používá slovo "status" k vyhledání chybných události **AzureDiagnostics**.</span><span class="sxs-lookup"><span data-stu-id="d1145-185">This example uses the word "status" to find failed events under **AzureDiagnostics**.</span></span>
   <span data-ttu-id="d1145-186">Zde filtr pro **status_s** je již vybrána.</span><span class="sxs-lookup"><span data-stu-id="d1145-186">Here the filter for **status_s** is already selected.</span></span>

   ![Vyberte filtr](media/logic-apps-monitor-your-logic-apps/log-search-add-filter.png)

8. <span data-ttu-id="d1145-188">V levém panelu, vyberte hodnota filtru, který chcete použít a vyberte **použít**.</span><span class="sxs-lookup"><span data-stu-id="d1145-188">In the left bar, select the filter value that you want to use, and choose **Apply**.</span></span>

   ![Vyberte hodnotu filtru, zvolte "Použít"](media/logic-apps-monitor-your-logic-apps/log-search-apply-filter.png)

9. <span data-ttu-id="d1145-190">Nyní se vraťte na dotaz, který vytváříte.</span><span class="sxs-lookup"><span data-stu-id="d1145-190">Now return to the query that you're building.</span></span> <span data-ttu-id="d1145-191">Dotaz se aktualizuje se vybraný filtr a hodnotou.</span><span class="sxs-lookup"><span data-stu-id="d1145-191">Your query is updated with your selected filter and value.</span></span> <span data-ttu-id="d1145-192">Předchozí výsledky jsou nyní příliš filtrovány.</span><span class="sxs-lookup"><span data-stu-id="d1145-192">Your previous results are now filtered too.</span></span>

   ![Vrátí do dotazu s filtrované výsledky.](media/logic-apps-monitor-your-logic-apps/log-search-query-filtered-results.png)

10. <span data-ttu-id="d1145-194">Pokud chcete uložit dotazu pro budoucí použití, zvolte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d1145-194">To save your query for future use, choose **Save**.</span></span> <span data-ttu-id="d1145-195">Další informace [jak uložit dotazu](../logic-apps/logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md#save-oms-query).</span><span class="sxs-lookup"><span data-stu-id="d1145-195">Learn [how to save your query](../logic-apps/logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md#save-oms-query).</span></span>

<a name="extend-diagnostic-data"></a>

## <a name="extend-how-and-where-you-use-diagnostic-data-with-other-services"></a><span data-ttu-id="d1145-196">Rozšíření jak a kde používáte diagnostických dat s jinými službami</span><span class="sxs-lookup"><span data-stu-id="d1145-196">Extend how and where you use diagnostic data with other services</span></span>

<span data-ttu-id="d1145-197">Společně s Azure Log Analytics můžete rozšířit použití aplikace logiky diagnostických dat s jinými službami Azure, například:</span><span class="sxs-lookup"><span data-stu-id="d1145-197">Along with Azure Log Analytics, you can extend how you use your logic app's diagnostic data with other Azure services, for example:</span></span> 

* [<span data-ttu-id="d1145-198">Archiv, který Azure Diagnostics protokolů v úložišti Azure</span><span class="sxs-lookup"><span data-stu-id="d1145-198">Archive Azure Diagnostics Logs in Azure Storage</span></span>](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md)
* [<span data-ttu-id="d1145-199">Datový proud protokolů Azure Diagnostics do služby Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="d1145-199">Stream Azure Diagnostics Logs to Azure Event Hubs</span></span>](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md) 

<span data-ttu-id="d1145-200">Můžete pak monitorování pomocí telemetrie a analýzy z jiných služeb, jako například get v reálném čase [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) a [Power BI](../log-analytics/log-analytics-powerbi.md).</span><span class="sxs-lookup"><span data-stu-id="d1145-200">You can then get real-time monitoring by using telemetry and analytics from other services, like [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) and [Power BI](../log-analytics/log-analytics-powerbi.md).</span></span> <span data-ttu-id="d1145-201">Například:</span><span class="sxs-lookup"><span data-stu-id="d1145-201">For example:</span></span>

* [<span data-ttu-id="d1145-202">Datový proud dat ze služby Event Hubs do služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="d1145-202">Stream data from Event Hubs to Stream Analytics</span></span>](../stream-analytics/stream-analytics-define-inputs.md)
* [<span data-ttu-id="d1145-203">Analýza dat pomocí služby Stream Analytics a vytvořit řídicí panel analýzu v reálném čase v Power BI</span><span class="sxs-lookup"><span data-stu-id="d1145-203">Analyze streaming data with Stream Analytics and create a real-time analytics dashboard in Power BI</span></span>](../stream-analytics/stream-analytics-power-bi-dashboard.md)

<span data-ttu-id="d1145-204">V závislosti na možnosti, které chcete nastavit, ujistěte se, že jste první [vytvořit účet úložiště Azure](../storage/common/storage-create-storage-account.md) nebo [vytváření centra událostí Azure](../event-hubs/event-hubs-create.md).</span><span class="sxs-lookup"><span data-stu-id="d1145-204">Based on the options that you want set up, make sure that you first [create an Azure storage account](../storage/common/storage-create-storage-account.md) or [create an Azure event hub](../event-hubs/event-hubs-create.md).</span></span> <span data-ttu-id="d1145-205">Vyberte požadované možnosti pro které chcete poslat diagnostická data:</span><span class="sxs-lookup"><span data-stu-id="d1145-205">Then select the options for where you want to send diagnostic data:</span></span>

![Odesílání dat do úložiště Azure účet nebo události rozbočovače](./media/logic-apps-monitor-your-logic-apps/storage-account-event-hubs.png)

> [!NOTE]
> <span data-ttu-id="d1145-207">Období uchovávání se projeví pouze v případě, že ji rozhodnete použít účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="d1145-207">Retention periods apply only when you choose to use a storage account.</span></span>

<a name="add-azure-alerts"></a>

## <a name="set-up-alerts-for-your-logic-app"></a><span data-ttu-id="d1145-208">Nastavit výstrahy pro svou aplikaci logiky</span><span class="sxs-lookup"><span data-stu-id="d1145-208">Set up alerts for your logic app</span></span>

<span data-ttu-id="d1145-209">Chcete-li monitorovat konkrétní metriky nebo překročil prahové hodnoty pro svou aplikaci logiky, nastavte [výstrahy v Azure](../monitoring-and-diagnostics/monitoring-overview-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="d1145-209">To monitor specific metrics or exceeded thresholds for your logic app, set up [alerts in Azure](../monitoring-and-diagnostics/monitoring-overview-alerts.md).</span></span> <span data-ttu-id="d1145-210">Další informace o [metriky v Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="d1145-210">Learn about [metrics in Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).</span></span> 

<span data-ttu-id="d1145-211">Nastavit výstrahy bez [Azure Log Analytics](../log-analytics/log-analytics-overview.md), postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="d1145-211">To set up alerts without [Azure Log Analytics](../log-analytics/log-analytics-overview.md), follow these steps.</span></span> <span data-ttu-id="d1145-212">Pro pokročilejší kritéria výstrahy a akce [nastavení analýzy protokolů](#azure-diagnostics) příliš.</span><span class="sxs-lookup"><span data-stu-id="d1145-212">For more advanced alerts criteria and actions, [set up Log Analytics](#azure-diagnostics) too.</span></span>

1. <span data-ttu-id="d1145-213">V nabídce okno aplikace logiky v rámci **monitorování**, zvolte **diagnostiky** > **výstrah pravidla** > **přidat upozornění**jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="d1145-213">On the logic app blade menu, under **Monitoring**, choose **Diagnostics** > **Alert rules** > **Add alert** as shown here:</span></span>

   ![Přidání oznámení pro svou aplikaci logiky](media/logic-apps-monitor-your-logic-apps/set-up-alerts.png)

2. <span data-ttu-id="d1145-215">Na **přidání pravidla výstrahy** okně vytvořit upozornění, jak je znázorněno:</span><span class="sxs-lookup"><span data-stu-id="d1145-215">On the **Add an alert rule** blade, create your alert as shown:</span></span>

   1. <span data-ttu-id="d1145-216">V části **prostředků**, vyberte svou aplikaci logiky, pokud není vybrána.</span><span class="sxs-lookup"><span data-stu-id="d1145-216">Under **Resource**, select your logic app, if not already selected.</span></span> 
   2. <span data-ttu-id="d1145-217">Zadejte název a popis pro upozornění.</span><span class="sxs-lookup"><span data-stu-id="d1145-217">Give a name and description for your alert.</span></span>
   3. <span data-ttu-id="d1145-218">Vyberte **metrika** nebo událost, která chcete sledovat.</span><span class="sxs-lookup"><span data-stu-id="d1145-218">Select a **Metric** or event that you want to track.</span></span>
   4. <span data-ttu-id="d1145-219">Vyberte **podmínku**, zadejte **prahová hodnota** pro metriku, vyberte **období** pro monitorování tato metrika.</span><span class="sxs-lookup"><span data-stu-id="d1145-219">Select a **Condition**, specify a **Threshold** for the metric, and select the **Period** for monitoring this metric.</span></span>
   5. <span data-ttu-id="d1145-220">Vyberte, jestli se mají odesílat e-mailu pro výstrahy.</span><span class="sxs-lookup"><span data-stu-id="d1145-220">Select whether to send mail for the alert.</span></span> 
   6. <span data-ttu-id="d1145-221">Zadejte jiné e-mailové adresy pro odesílání upozornění.</span><span class="sxs-lookup"><span data-stu-id="d1145-221">Specify any other email addresses for sending the alert.</span></span> 
   <span data-ttu-id="d1145-222">Můžete také zadat adresu URL webhooku, kde chcete odeslat výstrahu.</span><span class="sxs-lookup"><span data-stu-id="d1145-222">You can also specify a webhook URL where you want to send the alert.</span></span>

   <span data-ttu-id="d1145-223">Například toto pravidlo, odešle výstrahu, pokud pět nebo další spustí selhání za hodinu:</span><span class="sxs-lookup"><span data-stu-id="d1145-223">For example, this rule sends an alert when five or more runs fail in an hour:</span></span>

   ![Vytvoření metriky pravidlo výstrahy](media/logic-apps-monitor-your-logic-apps/create-alert-rule.png)

> [!TIP]
> <span data-ttu-id="d1145-225">Pokud chcete spustit aplikaci logiky z výstrahy, můžete zahrnout [aktivační událost požadavku](../connectors/connectors-native-reqres.md) v pracovním postupu, která vám umožňuje provádět úlohy, jako tyto příklady:</span><span class="sxs-lookup"><span data-stu-id="d1145-225">To run a logic app from an alert, you can include the [request trigger](../connectors/connectors-native-reqres.md) in your workflow, which lets you perform tasks like these examples:</span></span>
> 
> * [<span data-ttu-id="d1145-226">POST k Slack</span><span class="sxs-lookup"><span data-stu-id="d1145-226">Post to Slack</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
> * [<span data-ttu-id="d1145-227">Odeslat jako text</span><span class="sxs-lookup"><span data-stu-id="d1145-227">Send a text</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
> * [<span data-ttu-id="d1145-228">Přidání zprávy do fronty</span><span class="sxs-lookup"><span data-stu-id="d1145-228">Add a message to a queue</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)

<a name="diagnostic-event-properties"></a>

## <a name="azure-diagnostics-event-settings-and-details"></a><span data-ttu-id="d1145-229">Nastavení Azure Diagnostics událostí a podrobnosti</span><span class="sxs-lookup"><span data-stu-id="d1145-229">Azure Diagnostics event settings and details</span></span>

<span data-ttu-id="d1145-230">Každý diagnostických událostí obsahuje podrobnosti o aplikaci logiky a tuto událost, například stav, čas spuštění, koncový čas a tak dále.</span><span class="sxs-lookup"><span data-stu-id="d1145-230">Each diagnostic event has details about your logic app and that event, for example, the status, start time, end time, and so on.</span></span> <span data-ttu-id="d1145-231">Chcete-li nastavit prostřednictvím kódu programu, monitorování, sledování a protokolování, organizační jednotky použít tyto podrobnosti s [REST API pro Azure Logic Apps](https://docs.microsoft.com/rest/api/logic) a [REST API pro Azure Diagnostics](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftlogicworkflows).</span><span class="sxs-lookup"><span data-stu-id="d1145-231">To programmatically set up monitoring, tracking, and logging, ou can use these details with the [REST API for Azure Logic Apps](https://docs.microsoft.com/rest/api/logic) and the [REST API for Azure Diagnostics](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftlogicworkflows).</span></span>

<span data-ttu-id="d1145-232">Například `ActionCompleted` událost má `clientTrackingId` a `trackedProperties` vlastnosti, které můžete použít pro sledování a monitorování:</span><span class="sxs-lookup"><span data-stu-id="d1145-232">For example, the `ActionCompleted` event has the `clientTrackingId` and `trackedProperties` properties that you can use for tracking and monitoring:</span></span>

``` json
{
    "time": "2016-07-09T17:09:54.4773148Z",
    "workflowId": "/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP",
    "resourceId": "/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP/RUNS/08587361146922712057/ACTIONS/HTTP",
    "category": "WorkflowRuntime",
    "level": "Information",
    "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
    "properties": {
        "$schema": "2016-06-01",
        "startTime": "2016-07-09T17:09:53.4336305Z",
        "endTime": "2016-07-09T17:09:53.5430281Z",
        "status": "Succeeded",
        "code": "OK",
        "resource": {
            "subscriptionId": "<subscription-ID>",
            "resourceGroupName": "MyResourceGroup",
            "workflowId": "cff00d5458f944d5a766f2f9ad142553",
            "workflowName": "MyLogicApp",
            "runId": "08587361146922712057",
            "location": "westus",
            "actionName": "Http"
        },
        "correlation": {
            "actionTrackingId": "e1931543-906d-4d1d-baed-dee72ddf1047",
            "clientTrackingId": "<my-custom-tracking-ID>"
        },
        "trackedProperties": {
            "myTrackedProperty": "<value>"
        }
    }
}
```

* <span data-ttu-id="d1145-233">`clientTrackingId`: Pokud není zadáno, Azure automaticky vygeneruje ID a korelaci událostí napříč spustit aplikace logiky, včetně všech vnořených pracovních postupů, se nazývají z aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="d1145-233">`clientTrackingId`: If not provided, Azure automatically generates this ID and correlates events across a logic app run, including any nested workflows that are called from the logic app.</span></span> <span data-ttu-id="d1145-234">Můžete ručně zadat toto ID z aktivační událost pomocí předání `x-ms-client-tracking-id` hlavička s vlastní hodnota ID v žádosti o aktivační události.</span><span class="sxs-lookup"><span data-stu-id="d1145-234">You can manually specify this ID from a trigger by passing a `x-ms-client-tracking-id` header with your custom ID value in the trigger request.</span></span> <span data-ttu-id="d1145-235">Můžete použít aktivační událost požadavku, triggeru protokolu HTTP nebo aktivační události webhooku.</span><span class="sxs-lookup"><span data-stu-id="d1145-235">You can use a request trigger, HTTP trigger, or webhook trigger.</span></span>

* <span data-ttu-id="d1145-236">`trackedProperties`: Chcete-li sledovat vstupů nebo výstupů v diagnostická data, můžete přidat sledovaných vlastnosti na akce v definici aplikace logiky JSON.</span><span class="sxs-lookup"><span data-stu-id="d1145-236">`trackedProperties`: To track inputs or outputs in diagnostics data, you can add tracked properties to actions in your logic app's JSON definition.</span></span> <span data-ttu-id="d1145-237">Sledované vlastnosti můžete sledovat jenom jednu akci vstupy a výstupy, ale můžete použít `correlation` vlastnosti události ke korelaci napříč akce v spustit.</span><span class="sxs-lookup"><span data-stu-id="d1145-237">Tracked properties can track only a single action's inputs and outputs, but you can use the `correlation` properties of events to correlate across actions in a run.</span></span>

  <span data-ttu-id="d1145-238">Chcete-li sledovat jednu nebo více vlastností, přidejte `trackedProperties` části a vlastnosti, které chcete definici akce.</span><span class="sxs-lookup"><span data-stu-id="d1145-238">To track one or more properties, add the `trackedProperties` section and the properties you want to the action definition.</span></span> <span data-ttu-id="d1145-239">Předpokládejme například, že chcete sledovat data jako "ID pořadí" v telemetrie:</span><span class="sxs-lookup"><span data-stu-id="d1145-239">For example, suppose you want to track data like an "order ID" in your telemetry:</span></span>

  ``` json
  "myAction": {
    "type": "http",
    "inputs": {
        "uri": "http://uri",
        "headers": {
            "Content-Type": "application/json"
        },
        "body": "@triggerBody()"
    },
    "trackedProperties": {
        "myActionHTTPStatusCode": "@action()['outputs']['statusCode']",
        "myActionHTTPValue": "@action()['outputs']['body']['<content>']",
        "transactionId": "@action()['inputs']['body']['<content>']"
    }
  }
  ```

## <a name="next-steps"></a><span data-ttu-id="d1145-240">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d1145-240">Next steps</span></span>

* [<span data-ttu-id="d1145-241">Vytvoření šablony pro nasazení aplikace logiky a správa verzí</span><span class="sxs-lookup"><span data-stu-id="d1145-241">Create templates for logic app deployment and release management</span></span>](../logic-apps/logic-apps-create-deploy-template.md)
* [<span data-ttu-id="d1145-242">Scénáře B2B s Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="d1145-242">B2B scenarios with Enterprise Integration Pack</span></span>](../logic-apps/logic-apps-enterprise-integration-overview.md)
* [<span data-ttu-id="d1145-243">Monitorování zpráv B2B</span><span class="sxs-lookup"><span data-stu-id="d1145-243">Monitor B2B messages</span></span>](../logic-apps/logic-apps-monitor-b2b-message.md)