---
title: "aaaQuery zpráv B2B v Operations Management Suite - Azure Logic Apps | Microsoft Docs"
description: "Vytvoření dotazů tootrack AS2, X 12 a EDIFACT zprávy v hello Operations Management Suite"
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: aee6644ff19add8f074ed5f1725db87b1d3b74b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="query-for-as2-x12-and-edifact-messages-in-hello-microsoft-operations-management-suite-oms"></a><span data-ttu-id="f4190-103">Dotaz pro AS2, X 12 a EDIFACT zprávy v hello Microsoft Operations Management Suite (OMS)</span><span class="sxs-lookup"><span data-stu-id="f4190-103">Query for AS2, X12, and EDIFACT messages in hello Microsoft Operations Management Suite (OMS)</span></span>

<span data-ttu-id="f4190-104">toofind hello AS2, X 12 a EDIFACT zprávy, které sledujete s [Azure Log Analytics](../log-analytics/log-analytics-overview.md) v hello [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md), můžete vytvořit dotazy, které jsou založené na konkrétní akce filtru kritéria.</span><span class="sxs-lookup"><span data-stu-id="f4190-104">toofind hello AS2, X12, or EDIFACT messages that you're tracking with [Azure Log Analytics](../log-analytics/log-analytics-overview.md) in hello [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md), you can create queries that filter actions based on specific criteria.</span></span> <span data-ttu-id="f4190-105">Například můžete najít na základě různých řízení konkrétní výměnu zpráv.</span><span class="sxs-lookup"><span data-stu-id="f4190-105">For example, you can find messages based on a specific interchange control number.</span></span>

## <a name="requirements"></a><span data-ttu-id="f4190-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f4190-106">Requirements</span></span>

* <span data-ttu-id="f4190-107">Aplikace logiky, který je nastavený s protokolování diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="f4190-107">A logic app that's set up with diagnostics logging.</span></span> <span data-ttu-id="f4190-108">Další informace [jak toocreate aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md) a [jak tooset protokolování pro tuto aplikaci logiky](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).</span><span class="sxs-lookup"><span data-stu-id="f4190-108">Learn [how toocreate a logic app](../logic-apps/logic-apps-create-a-logic-app.md) and [how tooset up logging for that logic app](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).</span></span>

* <span data-ttu-id="f4190-109">Integrace účet, který je nastavený s sledování a protokolování.</span><span class="sxs-lookup"><span data-stu-id="f4190-109">An integration account that's set up with monitoring and logging.</span></span> <span data-ttu-id="f4190-110">Další informace [jak toocreate účet integrace](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) a [jak tooset sledování a protokolování pro tento účet](../logic-apps/logic-apps-monitor-b2b-message.md).</span><span class="sxs-lookup"><span data-stu-id="f4190-110">Learn [how toocreate an integration account](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) and [how tooset up monitoring and logging for that account](../logic-apps/logic-apps-monitor-b2b-message.md).</span></span>

* <span data-ttu-id="f4190-111">Pokud jste to ještě neudělali, [publikování diagnostických dat tooLog Analytics](../logic-apps/logic-apps-track-b2b-messages-omsportal.md) a [nastavení sledování v OMS zpráv](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span><span class="sxs-lookup"><span data-stu-id="f4190-111">If you haven't already, [publish diagnostic data tooLog Analytics](../logic-apps/logic-apps-track-b2b-messages-omsportal.md) and [set up message tracking in OMS](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span></span>

> [!NOTE]
> <span data-ttu-id="f4190-112">Po splnění požadavků na předchozí hello, měli byste mít pracovního prostoru v hello [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f4190-112">After you've met hello previous requirements, you should have a workspace in hello [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md).</span></span> <span data-ttu-id="f4190-113">Měli byste použít hello stejný pracovní prostor OMS pro sledování vaší komunikace B2B v OMS.</span><span class="sxs-lookup"><span data-stu-id="f4190-113">You should use hello same OMS workspace for tracking your B2B communication in OMS.</span></span> 
>  
> <span data-ttu-id="f4190-114">Pokud nemáte pracovním prostorem OMS, přečtěte si [jak toocreate pracovním prostorem OMS](../log-analytics/log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="f4190-114">If you don't have an OMS workspace, learn [how toocreate an OMS workspace](../log-analytics/log-analytics-get-started.md).</span></span>

## <a name="create-message-queries-with-filters-in-hello-operations-management-suite-portal"></a><span data-ttu-id="f4190-115">Vytváření dotazů zprávu s filtry portálu hello Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="f4190-115">Create message queries with filters in hello Operations Management Suite portal</span></span>

<span data-ttu-id="f4190-116">Tento příklad ukazuje, jak můžete najít podle jejich výměnu řízení počtu zpráv.</span><span class="sxs-lookup"><span data-stu-id="f4190-116">This example shows how you can find messages based on their interchange control number.</span></span>

> [!TIP] 
> <span data-ttu-id="f4190-117">Pokud znáte název pracovní prostor OMS, přejděte tooyour prostoru Domovská stránka (`https://{your-workspace-name}.portal.mms.microsoft.com`) a začít od kroku 4.</span><span class="sxs-lookup"><span data-stu-id="f4190-117">If you know your OMS workspace name, go tooyour workspace home page (`https://{your-workspace-name}.portal.mms.microsoft.com`), and start at Step 4.</span></span> <span data-ttu-id="f4190-118">Jinak začněte v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="f4190-118">Otherwise, start at Step 1.</span></span>

1. <span data-ttu-id="f4190-119">V hello [portál Azure](https://portal.azure.com), zvolte **více služeb**.</span><span class="sxs-lookup"><span data-stu-id="f4190-119">In hello [Azure portal](https://portal.azure.com), choose **More Services**.</span></span> <span data-ttu-id="f4190-120">Vyhledejte "analýzy protokolů" a potom vyberte **analýzy protokolů** jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="f4190-120">Search for "log analytics", and then choose **Log Analytics** as shown here:</span></span>

   ![Najít analýzy protokolů](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/browseloganalytics.png)

2. <span data-ttu-id="f4190-122">V části **analýzy protokolů**, najděte a vyberte pracovní prostor OMS.</span><span class="sxs-lookup"><span data-stu-id="f4190-122">Under **Log Analytics**, find and select your OMS workspace.</span></span>

   ![Vyberte pracovní prostor OMS](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/selectla.png)

3. <span data-ttu-id="f4190-124">V části **správy**, zvolte **portálu OMS**.</span><span class="sxs-lookup"><span data-stu-id="f4190-124">Under **Management**, choose **OMS Portal**.</span></span>

   ![Zvolte portálu OMS](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/omsportalpage.png)

4. <span data-ttu-id="f4190-126">Na domovské stránce OMS, zvolte **hledání protokolů**.</span><span class="sxs-lookup"><span data-stu-id="f4190-126">On your OMS home page, choose **Log Search**.</span></span>

   ![Na domovské stránce OMS zvolte "Hledání protokolů"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch.png)

   <span data-ttu-id="f4190-128">-nebo-</span><span class="sxs-lookup"><span data-stu-id="f4190-128">-or-</span></span>

   ![V nabídce OMS hello zvolte "Hledání protokolů"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch-2.png)

5. <span data-ttu-id="f4190-130">Hello vyhledávacího pole zadejte pole, které chcete toofind a stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="f4190-130">In hello search box, enter a field that you want toofind, and press **Enter**.</span></span> <span data-ttu-id="f4190-131">Když začnete psát, ukazuje OMS je možné shody a operací, které můžete použít.</span><span class="sxs-lookup"><span data-stu-id="f4190-131">When you start typing, OMS shows you possible matches and operations that you can use.</span></span> <span data-ttu-id="f4190-132">Další informace o [jak toofind data v analýzy protokolů](../log-analytics/log-analytics-log-searches.md).</span><span class="sxs-lookup"><span data-stu-id="f4190-132">Learn more about [how toofind data in Log Analytics](../log-analytics/log-analytics-log-searches.md).</span></span>

   <span data-ttu-id="f4190-133">Tento příklad vyhledá pro události se **typ = AzureDiagnostics**.</span><span class="sxs-lookup"><span data-stu-id="f4190-133">This example searches for events with **Type=AzureDiagnostics**.</span></span>

   ![Začněte psát řetězec dotazu](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-start-query.png)

6. <span data-ttu-id="f4190-135">V levém panelu hello zvolte hello časový rámec, které chcete tooview.</span><span class="sxs-lookup"><span data-stu-id="f4190-135">In hello left bar, choose hello timeframe that you want tooview.</span></span> <span data-ttu-id="f4190-136">Zvolte tooadd tooyour dotazu filter, **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="f4190-136">tooadd a filter tooyour query, choose **+Add**.</span></span>

   ![Přidat filtr tooquery](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/query1.png)

7. <span data-ttu-id="f4190-138">V části **přidat filtry**, zadejte název filtru hello hello filtru chcete vyhledávat.</span><span class="sxs-lookup"><span data-stu-id="f4190-138">Under **Add Filters**, enter hello filter name so you can find hello filter you want.</span></span> <span data-ttu-id="f4190-139">Vyberte filtr hello a zvolte **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="f4190-139">Select hello filter, and choose **+Add**.</span></span>

   <span data-ttu-id="f4190-140">toofind hello výměnu řízení číslo, tento příklad hello slovo "výměnu" vyhledá a vybere **event_record_messageProperties_interchangeControlNumber_s** jako filtr hello.</span><span class="sxs-lookup"><span data-stu-id="f4190-140">toofind hello interchange control number, this example searches for hello word "interchange", and selects **event_record_messageProperties_interchangeControlNumber_s** as hello filter.</span></span>

   ![Vyberte filtr](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-add-filter.png)

9. <span data-ttu-id="f4190-142">V levém panelu hello vyberte hello filtru hodnotu, kterou chcete toouse a zvolte **použít**.</span><span class="sxs-lookup"><span data-stu-id="f4190-142">In hello left bar, select hello filter value that you want toouse, and choose **Apply**.</span></span>

   <span data-ttu-id="f4190-143">Tento příklad vybere hello výměnu řízení číslo pro zprávy hello, kterou chceme.</span><span class="sxs-lookup"><span data-stu-id="f4190-143">This example selects hello interchange control number for hello messages we want.</span></span>

   ![Vyberte hodnotu filtru](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-select-filter-value.png)

10. <span data-ttu-id="f4190-145">Nyní se vraťte toohello dotaz, který vytváříte.</span><span class="sxs-lookup"><span data-stu-id="f4190-145">Now return toohello query that you're building.</span></span> <span data-ttu-id="f4190-146">Dotaz se aktualizovala se vybraný filtr událostí a hodnotou.</span><span class="sxs-lookup"><span data-stu-id="f4190-146">Your query has been updated with your selected filter event and value.</span></span> <span data-ttu-id="f4190-147">Předchozí výsledky jsou nyní příliš filtrovány.</span><span class="sxs-lookup"><span data-stu-id="f4190-147">Your previous results are now filtered too.</span></span>

    ![Vrátí tooyour dotaz s filtrované výsledky](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-filtered-results.png)

<a name="save-oms-query"></a>

## <a name="save-your-query-for-future-use"></a><span data-ttu-id="f4190-149">Své uložit pro budoucí použití</span><span class="sxs-lookup"><span data-stu-id="f4190-149">Save your query for future use</span></span>

1. <span data-ttu-id="f4190-150">Z dotazu na hello **hledání protokolů** vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f4190-150">From your query on hello **Log Search** page, choose **Save**.</span></span> <span data-ttu-id="f4190-151">Zadejte název dotazu, vyberte kategorii a zvolte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f4190-151">Give your query a name, select a category, and choose **Save**.</span></span>

   ![Dejte dotazu, název a kategorii](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-save.png)

2. <span data-ttu-id="f4190-153">tooview dotazu, zvolte **Oblíbené**.</span><span class="sxs-lookup"><span data-stu-id="f4190-153">tooview your query, choose **Favorites**.</span></span>

   ![Zvolte "Oblíbené"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-favorites.png)

3. <span data-ttu-id="f4190-155">V části **uložená hledání**, vyberte svůj dotaz tak, aby hello výsledky lze zobrazit.</span><span class="sxs-lookup"><span data-stu-id="f4190-155">Under **Saved Searches**, select your query so that you can view hello results.</span></span> <span data-ttu-id="f4190-156">dotaz hello upravit dotaz hello tooupdate vyhledávat odlišné výsledky.</span><span class="sxs-lookup"><span data-stu-id="f4190-156">tooupdate hello query so you can find different results, edit hello query.</span></span>

   ![Vyberte dotaz](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-find-favorites.png)

## <a name="find-and-run-saved-queries-in-hello-operations-management-suite-portal"></a><span data-ttu-id="f4190-158">Najít a spustit uložené dotazy na portálu služby Operations Management Suite hello</span><span class="sxs-lookup"><span data-stu-id="f4190-158">Find and run saved queries in hello Operations Management Suite portal</span></span>

1. <span data-ttu-id="f4190-159">Otevřete domovskou stránku pracovní prostor OMS (`https://{your-workspace-name}.portal.mms.microsoft.com`) a zvolte **hledání protokolů**.</span><span class="sxs-lookup"><span data-stu-id="f4190-159">Open your OMS workspace home page (`https://{your-workspace-name}.portal.mms.microsoft.com`), and choose **Log Search**.</span></span>

   ![Na domovské stránce OMS zvolte "Hledání protokolů"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch.png)

   <span data-ttu-id="f4190-161">-nebo-</span><span class="sxs-lookup"><span data-stu-id="f4190-161">-or-</span></span>

   ![V nabídce OMS hello zvolte "Hledání protokolů"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch-2.png)

2. <span data-ttu-id="f4190-163">Na hello **hledání protokolů** – Domovská stránka, zvolte **Oblíbené**.</span><span class="sxs-lookup"><span data-stu-id="f4190-163">On hello **Log Search** home page, choose **Favorites**.</span></span>

   ![Zvolte "Oblíbené"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-favorites.png)

3. <span data-ttu-id="f4190-165">V části **uložená hledání**, vyberte svůj dotaz tak, aby hello výsledky lze zobrazit.</span><span class="sxs-lookup"><span data-stu-id="f4190-165">Under **Saved Searches**, select your query so that you can view hello results.</span></span> <span data-ttu-id="f4190-166">dotaz hello upravit dotaz hello tooupdate vyhledávat odlišné výsledky.</span><span class="sxs-lookup"><span data-stu-id="f4190-166">tooupdate hello query so you can find different results, edit hello query.</span></span>

   ![Vyberte dotaz](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-find-favorites.png)

## <a name="next-steps"></a><span data-ttu-id="f4190-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f4190-168">Next steps</span></span>

* [<span data-ttu-id="f4190-169">Schémata sledování AS2</span><span class="sxs-lookup"><span data-stu-id="f4190-169">AS2 tracking schemas</span></span>](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [<span data-ttu-id="f4190-170">Schémata sledování X12</span><span class="sxs-lookup"><span data-stu-id="f4190-170">X12 tracking schemas</span></span>](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [<span data-ttu-id="f4190-171">Vlastní sledování schémata</span><span class="sxs-lookup"><span data-stu-id="f4190-171">Custom tracking schemas</span></span>](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)