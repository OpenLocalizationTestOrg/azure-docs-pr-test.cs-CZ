---
title: "nastavení protokolování aaaCheck stav a získat upozornění – Azure Logic Apps | Microsoft Docs"
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
ms.openlocfilehash: 81f186e11a669b710f4c06089597eb5a76f7a44e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-status-set-up-diagnostics-logging-and-turn-on-alerts-for-azure-logic-apps"></a><span data-ttu-id="cb3b4-103">Monitorování stavu, nastavit protokolování diagnostiky a zapnutí výstrah pro Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="cb3b4-103">Monitor status, set up diagnostics logging, and turn on alerts for Azure Logic Apps</span></span>

<span data-ttu-id="cb3b4-104">Po jste [vytvořte a spusťte aplikaci logiky](../logic-apps/logic-apps-create-a-logic-app.md), můžete zkontrolovat historii spustí, historie aktivační událost, stavu a výkonu.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-104">After you [create and run a logic app](../logic-apps/logic-apps-create-a-logic-app.md), you can check its runs history, trigger history, status, and performance.</span></span> <span data-ttu-id="cb3b4-105">Nastavení pro sledování událostí v reálném čase a bohatší ladění, [protokolování diagnostiky](#azure-diagnostics) pro svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-105">For real-time event monitoring and richer debugging, set up [diagnostics logging](#azure-diagnostics) for your logic app.</span></span> <span data-ttu-id="cb3b4-106">Tímto způsobem můžete [najít a zobrazit události](#find-events), jako jsou aktivační události, spuštění události a události akce.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-106">That way, you can [find and view events](#find-events), like trigger events, run events, and action events.</span></span> <span data-ttu-id="cb3b4-107">Můžete také použít tuto [diagnostická data s jinými službami](#extend-diagnostic-data), jako jsou služby Azure Storage a Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-107">You can also use this [diagnostics data with other services](#extend-diagnostic-data), like Azure Storage and Azure Event Hubs.</span></span> 

<span data-ttu-id="cb3b4-108">nastavit tooget oznámení o selhání nebo jiné možné problémy [výstrahy](#add-azure-alerts).</span><span class="sxs-lookup"><span data-stu-id="cb3b4-108">tooget notifications about failures or other possible problems, set up [alerts](#add-azure-alerts).</span></span> <span data-ttu-id="cb3b4-109">Například můžete vytvořit výstrahu, která zjistí "při více než pět spustí nezdaří za hodinu."</span><span class="sxs-lookup"><span data-stu-id="cb3b4-109">For example, you can create an alert that detects "when more than five runs fail in an hour."</span></span> <span data-ttu-id="cb3b4-110">Můžete také nastavit monitorování, sledování a protokolování programově pomocí [Azure Diagnostics událostí nastavení a vlastností](#diagnostic-event-properties).</span><span class="sxs-lookup"><span data-stu-id="cb3b4-110">You can also set up monitoring, tracking, and logging programmatically by using [Azure Diagnostics event settings and properties](#diagnostic-event-properties).</span></span>

## <a name="view-runs-and-trigger-history-for-your-logic-app"></a><span data-ttu-id="cb3b4-111">Spustí zobrazení a historii aktivační událost pro svou aplikaci logiky</span><span class="sxs-lookup"><span data-stu-id="cb3b4-111">View runs and trigger history for your logic app</span></span>

1. <span data-ttu-id="cb3b4-112">toofind aplikace logiky v hello [portál Azure](https://portal.azure.com), na hello hlavní nabídky Azure, zvolte **další služby**.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-112">toofind your logic app in hello [Azure portal](https://portal.azure.com), on hello main Azure menu, choose **More services**.</span></span> <span data-ttu-id="cb3b4-113">Hello vyhledávacího pole, vyhledejte "aplikace logiky" a zvolte **Logic apps**.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-113">In hello search box, find "logic apps", and choose **Logic apps**.</span></span>

   ![Najít aplikaci logiky](./media/logic-apps-monitor-your-logic-apps/find-your-logic-app.png)

   <span data-ttu-id="cb3b4-115">Hello portál Azure obsahuje všechny hello logiku aplikace, které jsou spojeny s předplatným Azure.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-115">hello Azure portal shows all hello logic apps that are associated with your Azure subscription.</span></span> 

2. <span data-ttu-id="cb3b4-116">Vyberte svou aplikaci logiky, a potom vyberte **přehled**.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-116">Select your logic app, then choose **Overview**.</span></span>

   <span data-ttu-id="cb3b4-117">Hello portál Azure zobrazuje hello spustí historie a historie aktivační událost pro svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-117">hello Azure portal shows hello runs history and trigger history for your logic app.</span></span> <span data-ttu-id="cb3b4-118">Například:</span><span class="sxs-lookup"><span data-stu-id="cb3b4-118">For example:</span></span>

   ![Spuštění aplikace logiky historie historie a aktivační události](media/logic-apps-monitor-your-logic-apps/overview.png)

   * <span data-ttu-id="cb3b4-120">**Spustí historie** ukazuje všechny hello spuštění pro svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-120">**Runs history** shows all hello runs for your logic app.</span></span> 
   * <span data-ttu-id="cb3b4-121">**Aktivovat historie** zobrazuje všechny aktivity hello aktivační událost pro svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-121">**Trigger History** shows all hello trigger activity for your logic app.</span></span>

   <span data-ttu-id="cb3b4-122">Popis stavu najdete v tématu [řešení aplikace logiky](../logic-apps/logic-apps-diagnosing-failures.md).</span><span class="sxs-lookup"><span data-stu-id="cb3b4-122">For status descriptions, see [Troubleshoot your logic app](../logic-apps/logic-apps-diagnosing-failures.md).</span></span>

   > [!TIP]
   > <span data-ttu-id="cb3b4-123">Pokud nenajdete hello data, která očekáváte, na panelu nástrojů hello, zvolte **aktualizovat**.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-123">If you don't find hello data that you expect, on hello toolbar, choose **Refresh**.</span></span>

3. <span data-ttu-id="cb3b4-124">tooview hello kroky v části z konkrétní spuštění, **spouští historie**, vyberte, které používají.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-124">tooview hello steps from a specific run, under **Runs history**, select that run.</span></span> 

   <span data-ttu-id="cb3b4-125">zobrazení monitorování Hello uvádí každý krok v tom, že spustit.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-125">hello monitor view shows each step in that run.</span></span> <span data-ttu-id="cb3b4-126">Například:</span><span class="sxs-lookup"><span data-stu-id="cb3b4-126">For example:</span></span>

   ![Akce pro konkrétní spustit](media/logic-apps-monitor-your-logic-apps/monitor-view-updated.png)

4. <span data-ttu-id="cb3b4-128">Vyberte další podrobnosti o hello spustit, tooget **podrobnosti o spuštění**.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-128">tooget more details about hello run, choose **Run Details**.</span></span> <span data-ttu-id="cb3b4-129">Tyto informace shrnuje kroky hello, stav, vstupy a výstupy pro hello spusťte.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-129">This information summarizes hello steps, status, inputs, and outputs for hello run.</span></span> 

   ![Zvolte "Spustit podrobnosti"](media/logic-apps-monitor-your-logic-apps/run-details.png)

   <span data-ttu-id="cb3b4-131">Například můžete získat hello spustit na **ID korelace**, což může být nutné při použití hello [REST API pro Logic Apps](https://docs.microsoft.com/rest/api/logic).</span><span class="sxs-lookup"><span data-stu-id="cb3b4-131">For example, you can get hello run's **Correlation ID**, which you might need when you use hello [REST API for Logic Apps](https://docs.microsoft.com/rest/api/logic).</span></span>

5. <span data-ttu-id="cb3b4-132">tooget podrobnosti o konkrétní krok, vyberte tento krok.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-132">tooget details about a specific step, choose that step.</span></span> <span data-ttu-id="cb3b4-133">Nyní můžete zkontrolovat podrobnosti, třeba vstupy, výstupy a všechny chyby, které pro tento krok se stalo.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-133">You can now review details like inputs, outputs, and any errors that happened for that step.</span></span> <span data-ttu-id="cb3b4-134">Například:</span><span class="sxs-lookup"><span data-stu-id="cb3b4-134">For example:</span></span>

   ![Podrobnosti o krok](media/logic-apps-monitor-your-logic-apps/monitor-view-details.png)
   
   > [!NOTE]
   > <span data-ttu-id="cb3b4-136">Všechny podrobnosti runtime a událostí, jsou šifrovány ve hello služba Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-136">All runtime details and events are encrypted within hello Logic Apps service.</span></span> <span data-ttu-id="cb3b4-137">Budou se dešifrují jenom v případě, že uživatel požádá o tooview tato data.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-137">They are decrypted only when a user requests tooview that data.</span></span> <span data-ttu-id="cb3b4-138">Můžete také ovládat přístup toothese události s [řízení řízení přístupu (RBAC)](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="cb3b4-138">You can also control access toothese events with [Azure Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-what-is.md).</span></span>

6. <span data-ttu-id="cb3b4-139">tooget podrobnosti o konkrétní aktivační událost, přejděte zpět toohello **přehled** podokně.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-139">tooget details about a specific trigger event, go back toohello **Overview** pane.</span></span> <span data-ttu-id="cb3b4-140">V části **aktivovat historie**vyberte hello aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-140">Under **Trigger history**, select hello trigger event.</span></span> <span data-ttu-id="cb3b4-141">Můžete teď zkontrolujte podrobnosti jako vstupy a výstupy, například:</span><span class="sxs-lookup"><span data-stu-id="cb3b4-141">You can now review details like inputs and outputs, for example:</span></span>

   ![Podrobnosti o aktivační události výstup](media/logic-apps-monitor-your-logic-apps/trigger-details.png)

<a name="azure-diagnostics"></a>

## <a name="turn-on-diagnostics-logging-for-your-logic-app"></a><span data-ttu-id="cb3b4-143">Zapněte diagnostiku protokolování pro svou aplikaci logiky</span><span class="sxs-lookup"><span data-stu-id="cb3b4-143">Turn on diagnostics logging for your logic app</span></span>

<span data-ttu-id="cb3b4-144">Pro širší ladění s podrobnostmi runtime a události, můžete nastavit protokolování s diagnostiky [Azure Log Analytics](../log-analytics/log-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="cb3b4-144">For richer debugging with runtime details and events, you can set up diagnostics logging with [Azure Log Analytics](../log-analytics/log-analytics-overview.md).</span></span> <span data-ttu-id="cb3b4-145">Analýzy protokolů je služba v [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) , sleduje vaše cloudové a místní prostředí toohelp udržování své dostupnosti a výkonu.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-145">Log Analytics is a service in [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) that monitors your cloud and on-premises environments toohelp you maintain their availability and performance.</span></span> 

<span data-ttu-id="cb3b4-146">Než začnete, je nutné toohave pracovním prostorem OMS.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-146">Before you start, you need toohave an OMS workspace.</span></span> <span data-ttu-id="cb3b4-147">Další informace [jak toocreate pracovním prostorem OMS](../log-analytics/log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="cb3b4-147">Learn [how toocreate an OMS workspace](../log-analytics/log-analytics-get-started.md).</span></span>

1. <span data-ttu-id="cb3b4-148">V hello [portál Azure](https://portal.azure.com), najděte a vyberte svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-148">In hello [Azure portal](https://portal.azure.com), find and select your logic app.</span></span> 

2. <span data-ttu-id="cb3b4-149">V nabídce okno aplikace logiky hello pod **monitorování**, zvolte **diagnostiky** > **nastavení pro diagnostiku**.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-149">On hello logic app blade menu, under **Monitoring**, choose **Diagnostics** > **Diagnostic Settings**.</span></span>

   ![Přejděte na nastavení pro diagnostiku tooMonitoring, diagnostiky,](media/logic-apps-monitor-your-logic-apps/logic-app-diagnostics.png)

3. <span data-ttu-id="cb3b4-151">V části **nastavení diagnostiky**, zvolte **na**.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-151">Under **Diagnostics settings**, choose **On**.</span></span>

   ![Zapnout diagnostické protokoly](media/logic-apps-monitor-your-logic-apps/turn-on-diagnostics-logic-app.png)

4. <span data-ttu-id="cb3b4-153">Nyní vyberte kategorii pracovní prostor a událostí OMS hello pro protokolování, jak je znázorněno:</span><span class="sxs-lookup"><span data-stu-id="cb3b4-153">Now select hello OMS workspace and event category for logging as shown:</span></span>

   1. <span data-ttu-id="cb3b4-154">Vyberte **odeslat tooLog Analytics**.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-154">Select **Send tooLog Analytics**.</span></span> 
   2. <span data-ttu-id="cb3b4-155">V části **analýzy protokolů**, zvolte **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-155">Under **Log Analytics**, choose **Configure**.</span></span> 
   3. <span data-ttu-id="cb3b4-156">V části **pracovních prostorů OMS**, vyberte hello OMS prostoru toouse pro protokolování.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-156">Under **OMS Workspaces**, select hello OMS workspace toouse for logging.</span></span>
   4. <span data-ttu-id="cb3b4-157">V části **protokolu**, vyberte hello **modul runtime pracovního postupu** kategorie.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-157">Under **Log**, select hello **WorkflowRuntime** category.</span></span>
   5. <span data-ttu-id="cb3b4-158">Zvolte metriky interval hello.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-158">Choose hello metric interval.</span></span>
   6. <span data-ttu-id="cb3b4-159">Až budete hotoví, zvolte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-159">When you're done, choose **Save**.</span></span>

   ![Vyberte pracovní prostor OMS a dat pro protokolování](media/logic-apps-monitor-your-logic-apps/send-diagnostics-data-log-analytics-workspace.png)

<span data-ttu-id="cb3b4-161">Teď můžete najít události a dalších dat pro aktivační události, spustit události a události akce.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-161">Now, you can find events and other data for trigger events, run events, and action events.</span></span>

<a name="find-events"></a>

## <a name="find-events-and-data-for-your-logic-app"></a><span data-ttu-id="cb3b4-162">Najít události a data pro svou aplikaci logiky</span><span class="sxs-lookup"><span data-stu-id="cb3b4-162">Find events and data for your logic app</span></span>

<span data-ttu-id="cb3b4-163">toofind a zobrazení událostí v aplikaci logiky, jako jsou aktivační události, spustit události a události akce, postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-163">toofind and view events in your logic app, like trigger events, run events, and action events, follow these steps.</span></span>

1. <span data-ttu-id="cb3b4-164">V hello [portál Azure](https://portal.azure.com), zvolte **více služeb**.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-164">In hello [Azure portal](https://portal.azure.com), choose **More Services**.</span></span> <span data-ttu-id="cb3b4-165">Vyhledejte "analýzy protokolů" a pak zvolte **analýzy protokolů** jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="cb3b4-165">Search for "log analytics", then choose **Log Analytics** as shown here:</span></span>

   ![Zvolte "Analýzy protokolů"](media/logic-apps-monitor-your-logic-apps/browseloganalytics.png)

2. <span data-ttu-id="cb3b4-167">V části **analýzy protokolů**, najděte a vyberte pracovní prostor OMS.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-167">Under **Log Analytics**, find and select your OMS workspace.</span></span> 

   ![Vyberte pracovní prostor OMS](media/logic-apps-monitor-your-logic-apps/selectla.png)

3. <span data-ttu-id="cb3b4-169">V části **správy**, zvolte **portálu OMS**.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-169">Under **Management**, choose **OMS Portal**.</span></span>

   ![Zvolte "Portál OMS"](media/logic-apps-monitor-your-logic-apps/omsportalpage.png)

4. <span data-ttu-id="cb3b4-171">Na domovské stránce OMS, zvolte **hledání protokolů**.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-171">On your OMS home page, choose **Log Search**.</span></span>

   ![Na domovské stránce OMS zvolte "Hledání protokolů"](media/logic-apps-monitor-your-logic-apps/logsearch.png)

   <span data-ttu-id="cb3b4-173">-nebo-</span><span class="sxs-lookup"><span data-stu-id="cb3b4-173">-or-</span></span>

   ![V nabídce OMS hello zvolte "Hledání protokolů"](media/logic-apps-monitor-your-logic-apps/logsearch-2.png)

5. <span data-ttu-id="cb3b4-175">Hello vyhledávacího pole zadejte pole, které chcete toofind a stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-175">In hello search box, specify a field that you want toofind, and press **Enter**.</span></span> <span data-ttu-id="cb3b4-176">Když začnete psát, ukazuje OMS je možné shody a operací, které můžete použít.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-176">When you start typing, OMS shows you possible matches and operations that you can use.</span></span> 

   <span data-ttu-id="cb3b4-177">Například toofind hello prvních 10 události, které se stalo, zadejte a vyberte tento vyhledávací dotaz: **kategorie = modul runtime pracovního postupu | top 10**</span><span class="sxs-lookup"><span data-stu-id="cb3b4-177">For example, toofind hello top 10 events that happened, enter and select this search query: **Category=WorkflowRuntime |top 10**</span></span>

   ![Zadejte hledaný řetězec](media/logic-apps-monitor-your-logic-apps/oms-start-query.png)

   <span data-ttu-id="cb3b4-179">Další informace o [jak toofind data v analýzy protokolů](../log-analytics/log-analytics-log-searches.md).</span><span class="sxs-lookup"><span data-stu-id="cb3b4-179">Learn more about [how toofind data in Log Analytics](../log-analytics/log-analytics-log-searches.md).</span></span>

6. <span data-ttu-id="cb3b4-180">Na stránce výsledky hello hello levém panelu, zvolte hello časový rámec, které chcete tooview.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-180">On hello results page, in hello left bar, choose hello timeframe that you want tooview.</span></span>
<span data-ttu-id="cb3b4-181">Zvolte dotaz přidáním filtru, toorefine **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-181">toorefine your query by adding a filter, choose **+Add**.</span></span>

   ![Vyberte časový rámec pro výsledky dotazu](media/logic-apps-monitor-your-logic-apps/query-results.png)

7. <span data-ttu-id="cb3b4-183">V části **přidat filtry**, zadejte název filtru hello hello filtru chcete vyhledávat.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-183">Under **Add Filters**, enter hello filter name so you can find hello filter you want.</span></span> <span data-ttu-id="cb3b4-184">Vyberte filtr hello a zvolte **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-184">Select hello filter, and choose **+Add**.</span></span>

   <span data-ttu-id="cb3b4-185">Tento příklad používá hello toofind "status" word se nezdařilo události **AzureDiagnostics**.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-185">This example uses hello word "status" toofind failed events under **AzureDiagnostics**.</span></span>
   <span data-ttu-id="cb3b4-186">Zde hello filtr **status_s** je již vybrána.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-186">Here hello filter for **status_s** is already selected.</span></span>

   ![Vyberte filtr](media/logic-apps-monitor-your-logic-apps/log-search-add-filter.png)

8. <span data-ttu-id="cb3b4-188">V levém panelu hello vyberte hello filtru hodnotu, kterou chcete toouse a zvolte **použít**.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-188">In hello left bar, select hello filter value that you want toouse, and choose **Apply**.</span></span>

   ![Vyberte hodnotu filtru, zvolte "Použít"](media/logic-apps-monitor-your-logic-apps/log-search-apply-filter.png)

9. <span data-ttu-id="cb3b4-190">Nyní se vraťte toohello dotaz, který vytváříte.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-190">Now return toohello query that you're building.</span></span> <span data-ttu-id="cb3b4-191">Dotaz se aktualizuje se vybraný filtr a hodnotou.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-191">Your query is updated with your selected filter and value.</span></span> <span data-ttu-id="cb3b4-192">Předchozí výsledky jsou nyní příliš filtrovány.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-192">Your previous results are now filtered too.</span></span>

   ![Vrátí tooyour dotaz s filtrované výsledky](media/logic-apps-monitor-your-logic-apps/log-search-query-filtered-results.png)

10. <span data-ttu-id="cb3b4-194">Zvolte dotaz pro budoucí použití, toosave **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-194">toosave your query for future use, choose **Save**.</span></span> <span data-ttu-id="cb3b4-195">Další informace [jak toosave dotazu](../logic-apps/logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md#save-oms-query).</span><span class="sxs-lookup"><span data-stu-id="cb3b4-195">Learn [how toosave your query](../logic-apps/logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md#save-oms-query).</span></span>

<a name="extend-diagnostic-data"></a>

## <a name="extend-how-and-where-you-use-diagnostic-data-with-other-services"></a><span data-ttu-id="cb3b4-196">Rozšíření jak a kde používáte diagnostických dat s jinými službami</span><span class="sxs-lookup"><span data-stu-id="cb3b4-196">Extend how and where you use diagnostic data with other services</span></span>

<span data-ttu-id="cb3b4-197">Společně s Azure Log Analytics můžete rozšířit použití aplikace logiky diagnostických dat s jinými službami Azure, například:</span><span class="sxs-lookup"><span data-stu-id="cb3b4-197">Along with Azure Log Analytics, you can extend how you use your logic app's diagnostic data with other Azure services, for example:</span></span> 

* [<span data-ttu-id="cb3b4-198">Archiv, který Azure Diagnostics protokolů v úložišti Azure</span><span class="sxs-lookup"><span data-stu-id="cb3b4-198">Archive Azure Diagnostics Logs in Azure Storage</span></span>](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md)
* [<span data-ttu-id="cb3b4-199">TooAzure datový proud protokolů diagnostiky Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="cb3b4-199">Stream Azure Diagnostics Logs tooAzure Event Hubs</span></span>](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md) 

<span data-ttu-id="cb3b4-200">Můžete pak monitorování pomocí telemetrie a analýzy z jiných služeb, jako například get v reálném čase [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) a [Power BI](../log-analytics/log-analytics-powerbi.md).</span><span class="sxs-lookup"><span data-stu-id="cb3b4-200">You can then get real-time monitoring by using telemetry and analytics from other services, like [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) and [Power BI](../log-analytics/log-analytics-powerbi.md).</span></span> <span data-ttu-id="cb3b4-201">Například:</span><span class="sxs-lookup"><span data-stu-id="cb3b4-201">For example:</span></span>

* [<span data-ttu-id="cb3b4-202">Datový proud dat ze služby Event Hubs tooStream Analytics</span><span class="sxs-lookup"><span data-stu-id="cb3b4-202">Stream data from Event Hubs tooStream Analytics</span></span>](../stream-analytics/stream-analytics-define-inputs.md)
* [<span data-ttu-id="cb3b4-203">Analýza dat pomocí služby Stream Analytics a vytvořit řídicí panel analýzu v reálném čase v Power BI</span><span class="sxs-lookup"><span data-stu-id="cb3b4-203">Analyze streaming data with Stream Analytics and create a real-time analytics dashboard in Power BI</span></span>](../stream-analytics/stream-analytics-power-bi-dashboard.md)

<span data-ttu-id="cb3b4-204">Podle hello možnosti, které chcete nastavit, ujistěte se, že jste první [vytvořit účet úložiště Azure](../storage/common/storage-create-storage-account.md) nebo [vytváření centra událostí Azure](../event-hubs/event-hubs-create.md).</span><span class="sxs-lookup"><span data-stu-id="cb3b4-204">Based on hello options that you want set up, make sure that you first [create an Azure storage account](../storage/common/storage-create-storage-account.md) or [create an Azure event hub](../event-hubs/event-hubs-create.md).</span></span> <span data-ttu-id="cb3b4-205">Potom vyberte možnosti hello místo toosend diagnostických dat:</span><span class="sxs-lookup"><span data-stu-id="cb3b4-205">Then select hello options for where you want toosend diagnostic data:</span></span>

![Odeslání dat tooAzure úložiště účet nebo události rozbočovače](./media/logic-apps-monitor-your-logic-apps/storage-account-event-hubs.png)

> [!NOTE]
> <span data-ttu-id="cb3b4-207">Období uchovávání použít pouze v případě, že zvolíte toouse účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-207">Retention periods apply only when you choose toouse a storage account.</span></span>

<a name="add-azure-alerts"></a>

## <a name="set-up-alerts-for-your-logic-app"></a><span data-ttu-id="cb3b4-208">Nastavit výstrahy pro svou aplikaci logiky</span><span class="sxs-lookup"><span data-stu-id="cb3b4-208">Set up alerts for your logic app</span></span>

<span data-ttu-id="cb3b4-209">konkrétní metriky toomonitor nebo překročil prahové hodnoty pro svou aplikaci logiky nastavit [výstrahy v Azure](../monitoring-and-diagnostics/monitoring-overview-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="cb3b4-209">toomonitor specific metrics or exceeded thresholds for your logic app, set up [alerts in Azure](../monitoring-and-diagnostics/monitoring-overview-alerts.md).</span></span> <span data-ttu-id="cb3b4-210">Další informace o [metriky v Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="cb3b4-210">Learn about [metrics in Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).</span></span> 

<span data-ttu-id="cb3b4-211">tooset si výstrahy bez [Azure Log Analytics](../log-analytics/log-analytics-overview.md), postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-211">tooset up alerts without [Azure Log Analytics](../log-analytics/log-analytics-overview.md), follow these steps.</span></span> <span data-ttu-id="cb3b4-212">Pro pokročilejší kritéria výstrahy a akce [nastavení analýzy protokolů](#azure-diagnostics) příliš.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-212">For more advanced alerts criteria and actions, [set up Log Analytics](#azure-diagnostics) too.</span></span>

1. <span data-ttu-id="cb3b4-213">V nabídce okno aplikace logiky hello pod **monitorování**, zvolte **diagnostiky** > **výstrah pravidla** > **přidat upozornění**jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="cb3b4-213">On hello logic app blade menu, under **Monitoring**, choose **Diagnostics** > **Alert rules** > **Add alert** as shown here:</span></span>

   ![Přidání oznámení pro svou aplikaci logiky](media/logic-apps-monitor-your-logic-apps/set-up-alerts.png)

2. <span data-ttu-id="cb3b4-215">Na hello **přidání pravidla výstrahy** okně vytvořit upozornění, jak je znázorněno:</span><span class="sxs-lookup"><span data-stu-id="cb3b4-215">On hello **Add an alert rule** blade, create your alert as shown:</span></span>

   1. <span data-ttu-id="cb3b4-216">V části **prostředků**, vyberte svou aplikaci logiky, pokud není vybrána.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-216">Under **Resource**, select your logic app, if not already selected.</span></span> 
   2. <span data-ttu-id="cb3b4-217">Zadejte název a popis pro upozornění.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-217">Give a name and description for your alert.</span></span>
   3. <span data-ttu-id="cb3b4-218">Vyberte **metrika** nebo událostí, které chcete tootrack.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-218">Select a **Metric** or event that you want tootrack.</span></span>
   4. <span data-ttu-id="cb3b4-219">Vyberte **podmínku**, zadejte **prahová hodnota** pro metriku hello a vyberte hello **období** pro monitorování tato metrika.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-219">Select a **Condition**, specify a **Threshold** for hello metric, and select hello **Period** for monitoring this metric.</span></span>
   5. <span data-ttu-id="cb3b4-220">Vyberte, zda toosend poštovní hello výstrahy.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-220">Select whether toosend mail for hello alert.</span></span> 
   6. <span data-ttu-id="cb3b4-221">Zadejte jiné e-mailové adresy pro odesílání upozornění hello.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-221">Specify any other email addresses for sending hello alert.</span></span> 
   <span data-ttu-id="cb3b4-222">Také můžete zadat adresu URL webhooku místo toosend hello výstrahy.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-222">You can also specify a webhook URL where you want toosend hello alert.</span></span>

   <span data-ttu-id="cb3b4-223">Například toto pravidlo, odešle výstrahu, pokud pět nebo další spustí selhání za hodinu:</span><span class="sxs-lookup"><span data-stu-id="cb3b4-223">For example, this rule sends an alert when five or more runs fail in an hour:</span></span>

   ![Vytvoření metriky pravidlo výstrahy](media/logic-apps-monitor-your-logic-apps/create-alert-rule.png)

> [!TIP]
> <span data-ttu-id="cb3b4-225">toorun aplikace logiky z výstrahy, můžete zahrnout hello [aktivační událost požadavku](../connectors/connectors-native-reqres.md) v pracovním postupu, která vám umožňuje provádět úlohy, jako tyto příklady:</span><span class="sxs-lookup"><span data-stu-id="cb3b4-225">toorun a logic app from an alert, you can include hello [request trigger](../connectors/connectors-native-reqres.md) in your workflow, which lets you perform tasks like these examples:</span></span>
> 
> * [<span data-ttu-id="cb3b4-226">POST tooSlack</span><span class="sxs-lookup"><span data-stu-id="cb3b4-226">Post tooSlack</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
> * [<span data-ttu-id="cb3b4-227">Odeslat jako text</span><span class="sxs-lookup"><span data-stu-id="cb3b4-227">Send a text</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
> * [<span data-ttu-id="cb3b4-228">Přidat tooa fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="cb3b4-228">Add a message tooa queue</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)

<a name="diagnostic-event-properties"></a>

## <a name="azure-diagnostics-event-settings-and-details"></a><span data-ttu-id="cb3b4-229">Nastavení Azure Diagnostics událostí a podrobnosti</span><span class="sxs-lookup"><span data-stu-id="cb3b4-229">Azure Diagnostics event settings and details</span></span>

<span data-ttu-id="cb3b4-230">Každý diagnostických událostí obsahuje podrobnosti o aplikaci logiky a tuto událost, například hello stav, čas spuštění, koncový čas a tak dále.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-230">Each diagnostic event has details about your logic app and that event, for example, hello status, start time, end time, and so on.</span></span> <span data-ttu-id="cb3b4-231">tooprogrammatically nastavení monitorování, sledování a protokolování, organizační jednotky můžete použít tyto podrobnosti s hello [REST API pro Azure Logic Apps](https://docs.microsoft.com/rest/api/logic) a hello [REST API pro Azure Diagnostics](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftlogicworkflows).</span><span class="sxs-lookup"><span data-stu-id="cb3b4-231">tooprogrammatically set up monitoring, tracking, and logging, ou can use these details with hello [REST API for Azure Logic Apps](https://docs.microsoft.com/rest/api/logic) and hello [REST API for Azure Diagnostics](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftlogicworkflows).</span></span>

<span data-ttu-id="cb3b4-232">Například hello `ActionCompleted` událost má hello `clientTrackingId` a `trackedProperties` vlastnosti, které můžete použít pro sledování a monitorování:</span><span class="sxs-lookup"><span data-stu-id="cb3b4-232">For example, hello `ActionCompleted` event has hello `clientTrackingId` and `trackedProperties` properties that you can use for tracking and monitoring:</span></span>

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

* <span data-ttu-id="cb3b4-233">`clientTrackingId`: Pokud není zadáno, Azure automaticky vygeneruje ID a korelaci událostí napříč spustit aplikace logiky, včetně všech vnořených pracovních postupů, se nazývají z aplikace logiky hello.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-233">`clientTrackingId`: If not provided, Azure automatically generates this ID and correlates events across a logic app run, including any nested workflows that are called from hello logic app.</span></span> <span data-ttu-id="cb3b4-234">Můžete ručně zadat toto ID z aktivační událost pomocí předání `x-ms-client-tracking-id` hlavička s vlastní hodnota ID v požadavek aktivace hello.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-234">You can manually specify this ID from a trigger by passing a `x-ms-client-tracking-id` header with your custom ID value in hello trigger request.</span></span> <span data-ttu-id="cb3b4-235">Můžete použít aktivační událost požadavku, triggeru protokolu HTTP nebo aktivační události webhooku.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-235">You can use a request trigger, HTTP trigger, or webhook trigger.</span></span>

* <span data-ttu-id="cb3b4-236">`trackedProperties`: tootrack vstupů nebo výstupů v diagnostická data, můžete přidat tooactions sledovaných vlastnosti v definici aplikace logiky JSON.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-236">`trackedProperties`: tootrack inputs or outputs in diagnostics data, you can add tracked properties tooactions in your logic app's JSON definition.</span></span> <span data-ttu-id="cb3b4-237">Sledované vlastnosti můžete sledovat jenom jednu akci vstupy a výstupy, ale můžete použít hello `correlation` vlastnosti události toocorrelate napříč akce v spustit.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-237">Tracked properties can track only a single action's inputs and outputs, but you can use hello `correlation` properties of events toocorrelate across actions in a run.</span></span>

  <span data-ttu-id="cb3b4-238">tootrack jednu nebo více vlastností, přidejte hello `trackedProperties` části a hello vlastnosti, které chcete, aby definice toohello akce.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-238">tootrack one or more properties, add hello `trackedProperties` section and hello properties you want toohello action definition.</span></span> <span data-ttu-id="cb3b4-239">Předpokládejme například, že chcete data tootrack jako "ID pořadí" v telemetrie:</span><span class="sxs-lookup"><span data-stu-id="cb3b4-239">For example, suppose you want tootrack data like an "order ID" in your telemetry:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="cb3b4-240">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cb3b4-240">Next steps</span></span>

* [<span data-ttu-id="cb3b4-241">Vytvoření šablony pro nasazení aplikace logiky a správa verzí</span><span class="sxs-lookup"><span data-stu-id="cb3b4-241">Create templates for logic app deployment and release management</span></span>](../logic-apps/logic-apps-create-deploy-template.md)
* [<span data-ttu-id="cb3b4-242">Scénáře B2B s Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="cb3b4-242">B2B scenarios with Enterprise Integration Pack</span></span>](../logic-apps/logic-apps-enterprise-integration-overview.md)
* [<span data-ttu-id="cb3b4-243">Monitorování zpráv B2B</span><span class="sxs-lookup"><span data-stu-id="cb3b4-243">Monitor B2B messages</span></span>](../logic-apps/logic-apps-monitor-b2b-message.md)