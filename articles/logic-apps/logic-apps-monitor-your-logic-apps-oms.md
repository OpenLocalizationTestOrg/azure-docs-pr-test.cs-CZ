---
title: "aaaMonitor a získání přehledy o aplikaci logiky spouští pomocí OMS - Azure Logic Apps | Microsoft Docs"
description: "Monitorování vaší logiku aplikace běží s insights tooget analýzy protokolů a Operations Management Suite (OMS) a bohatší ladění podrobnosti o řešení potíží a Diagnostika"
author: divyaswarnkar
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/9/2017
ms.author: LADocs; divswa
ms.openlocfilehash: a76fd6d1ff5c0010550be0f991514ce95f659fd6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-get-insights-about-logic-app-runs-with-operations-management-suite-oms-and-log-analytics"></a><span data-ttu-id="5ba16-103">Monitorovat a získat přehled o logiku aplikace běží s Operations Management Suite (OMS) a analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="5ba16-103">Monitor and get insights about logic app runs with Operations Management Suite (OMS) and Log Analytics</span></span>

<span data-ttu-id="5ba16-104">Pro monitorování a bohatší informace o ladění, můžete zapnout analýzy protokolů v hello stejnou dobu, po vytvoření aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="5ba16-104">For monitoring and richer debugging information, you can turn on Log Analytics at hello same time when you create a logic app.</span></span> <span data-ttu-id="5ba16-105">Analýzy protokolů poskytuje možnosti pro diagnostiku protokolování a monitorování pro svou aplikaci logiky spouští prostřednictvím portálu hello Operations Management Suite (OMS).</span><span class="sxs-lookup"><span data-stu-id="5ba16-105">Log Analytics provides diagnostics logging and monitoring for your logic app runs through hello Operations Management Suite (OMS) portal.</span></span> <span data-ttu-id="5ba16-106">Když přidáte tooOMS řešení správy Logic Apps hello, zobrazí agregovaný stav pro logiku aplikace běží a konkrétní podrobnosti, jako je stav, čas spuštění, opakované odeslání stavu a ID korelace.</span><span class="sxs-lookup"><span data-stu-id="5ba16-106">When you add hello Logic Apps Management solution tooOMS, you get aggregated status for your logic app runs and specific details like status, execution time, resubmission status, and correlation IDs.</span></span>

<span data-ttu-id="5ba16-107">Toto téma ukazuje, jak tooturn na analýzy protokolů nebo nainstalujte hello řešení pro správu aplikace logiky v OMS, můžete zobrazit události modulu runtime a spusťte data pro svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="5ba16-107">This topic shows how tooturn on Log Analytics or install hello Logic Apps Management solution in OMS so you can view runtime events and data for your logic app run.</span></span>

 > [!TIP]
 > <span data-ttu-id="5ba16-108">toomonitor existující logic apps, postupujte podle těchto kroků příliš [zapnout protokolování diagnostiky a odeslání logiky aplikace runtime data tooOMS](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).</span><span class="sxs-lookup"><span data-stu-id="5ba16-108">toomonitor your existing logic apps, follow these steps too [turn on diagnostic logging and send logic app runtime data tooOMS](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).</span></span>

## <a name="requirements"></a><span data-ttu-id="5ba16-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5ba16-109">Requirements</span></span>

<span data-ttu-id="5ba16-110">Než začnete, je nutné toohave pracovním prostorem OMS.</span><span class="sxs-lookup"><span data-stu-id="5ba16-110">Before you start, you need toohave an OMS workspace.</span></span> <span data-ttu-id="5ba16-111">Další informace [jak toocreate pracovním prostorem OMS](../log-analytics/log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="5ba16-111">Learn [how toocreate an OMS workspace](../log-analytics/log-analytics-get-started.md).</span></span> 

## <a name="turn-on-diagnostics-logging-when-creating-logic-apps"></a><span data-ttu-id="5ba16-112">Zapnutí protokolování diagnostiky při vytváření aplikací logiky</span><span class="sxs-lookup"><span data-stu-id="5ba16-112">Turn on diagnostics logging when creating logic apps</span></span>

1. <span data-ttu-id="5ba16-113">V [portál Azure](https://portal.azure.com), vytvoření aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="5ba16-113">In [Azure portal](https://portal.azure.com), create a logic app.</span></span> <span data-ttu-id="5ba16-114">Zvolte **nové** > **Enterprise integrace** > **aplikace logiky** > **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5ba16-114">Choose **New** > **Enterprise Integration** > **Logic App** > **Create**.</span></span>

   ![Vytvoření aplikace logiky](media/logic-apps-monitor-your-logic-apps-oms/find-logic-apps-azure.png)

2. <span data-ttu-id="5ba16-116">V hello **vytvořit aplikaci logiky** proveďte tyto úlohy, jak je znázorněno:</span><span class="sxs-lookup"><span data-stu-id="5ba16-116">In hello **Create logic app** page, perform these tasks as shown:</span></span>

   1. <span data-ttu-id="5ba16-117">Zadejte název pro svou aplikaci logiky a vyberte předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="5ba16-117">Provide a name for your logic app and select your Azure subscription.</span></span> 
   2. <span data-ttu-id="5ba16-118">Vytvořte nebo vyberte skupinu prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="5ba16-118">Create or select an Azure resource group.</span></span>
   3. <span data-ttu-id="5ba16-119">Nastavit **analýzy protokolů** příliš**na**.</span><span class="sxs-lookup"><span data-stu-id="5ba16-119">Set **Log Analytics** too**On**.</span></span> 
   <span data-ttu-id="5ba16-120">Pracovní prostor OMS hello vyberte místo, kam chcete příliš odesílat data pro svou aplikaci logiky spustí.</span><span class="sxs-lookup"><span data-stu-id="5ba16-120">Select hello OMS workspace where you want too send data for your logic app runs.</span></span> 
   4. <span data-ttu-id="5ba16-121">Až budete připraveni, zvolte **Pin toodashboard** > **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5ba16-121">When you're ready, choose **Pin toodashboard** > **Create**.</span></span>

      ![Vytvoření aplikace logiky](./media/logic-apps-monitor-your-logic-apps-oms/create-logic-app.png)

      <span data-ttu-id="5ba16-123">Po dokončení tohoto kroku, Azure vytvoří aplikace logiky, která je teď spojené s vaším pracovním prostorem OMS.</span><span class="sxs-lookup"><span data-stu-id="5ba16-123">After you finish this step, Azure creates your logic app, which is now associated with your OMS workspace.</span></span> 
      <span data-ttu-id="5ba16-124">Tento krok navíc také automaticky nainstaluje řešení pro správu aplikace logiky hello v pracovním prostoru OMS.</span><span class="sxs-lookup"><span data-stu-id="5ba16-124">Also, this step also automatically installs hello Logic Apps Management solution in your OMS workspace.</span></span>

3. <span data-ttu-id="5ba16-125">tooview v OMS, které běží vaše aplikace logiky [pokračovat v těchto krocích](#view-logic-app-runs-oms).</span><span class="sxs-lookup"><span data-stu-id="5ba16-125">tooview your logic app runs in OMS, [continue with these steps](#view-logic-app-runs-oms).</span></span>

## <a name="install-hello-logic-apps-management-solution-in-oms"></a><span data-ttu-id="5ba16-126">Nainstalujte řešení pro správu aplikace logiky hello v OMS</span><span class="sxs-lookup"><span data-stu-id="5ba16-126">Install hello Logic Apps Management solution in OMS</span></span>

<span data-ttu-id="5ba16-127">Pokud jste již zapnuli analýzy protokolů při vytvoření aplikace logiky, tento krok přeskočte.</span><span class="sxs-lookup"><span data-stu-id="5ba16-127">If you already turned on Log Analytics when you created your logic app, skip this step.</span></span> <span data-ttu-id="5ba16-128">Už máte řešení pro správu aplikace logiky hello nainstalované v OMS.</span><span class="sxs-lookup"><span data-stu-id="5ba16-128">You already have hello Logic Apps Management solution installed in OMS.</span></span>

1. <span data-ttu-id="5ba16-129">V hello [portál Azure](https://portal.azure.com), zvolte **více služeb**.</span><span class="sxs-lookup"><span data-stu-id="5ba16-129">In hello [Azure portal](https://portal.azure.com), choose **More Services**.</span></span> <span data-ttu-id="5ba16-130">Vyhledejte "analýzy protokolů" jako filtr a vyberte **analýzy protokolů** znázorněné:</span><span class="sxs-lookup"><span data-stu-id="5ba16-130">Search for "log analytics" as your filter, and choose **Log Analytics** as shown:</span></span>

   ![Zvolte "Analýzy protokolů"](media/logic-apps-monitor-your-logic-apps-oms/find-log-analytics.png)

2. <span data-ttu-id="5ba16-132">V části **analýzy protokolů**, najděte a vyberte pracovní prostor OMS.</span><span class="sxs-lookup"><span data-stu-id="5ba16-132">Under **Log Analytics**, find and select your OMS workspace.</span></span> 

   ![Vyberte pracovní prostor OMS](media/logic-apps-monitor-your-logic-apps-oms/select-logic-app.png)

3. <span data-ttu-id="5ba16-134">V části **správy**, zvolte **portálu OMS**.</span><span class="sxs-lookup"><span data-stu-id="5ba16-134">Under **Management**, choose **OMS Portal**.</span></span>

   ![Zvolte "Portál OMS"](media/logic-apps-monitor-your-logic-apps-oms/oms-portal-page.png)

4. <span data-ttu-id="5ba16-136">Na domovské stránce OMS Pokud se zobrazí nápis upgradu hello, zvolte hello banner, aby nejprve upgradovat vaším pracovním prostorem OMS.</span><span class="sxs-lookup"><span data-stu-id="5ba16-136">On your OMS homepage, if hello upgrade banner appears, choose hello banner so that you upgrade your OMS workspace first.</span></span> <span data-ttu-id="5ba16-137">Zvolte **řešení Galerie**.</span><span class="sxs-lookup"><span data-stu-id="5ba16-137">Then choose **Solutions Gallery**.</span></span>

   ![Zvolte "Galerie řešení"](media/logic-apps-monitor-your-logic-apps-oms/solutions-gallery.png)

5. <span data-ttu-id="5ba16-139">V části **všechna řešení**, najít a vyberte dlaždici hello hello **správy Logic Apps** řešení.</span><span class="sxs-lookup"><span data-stu-id="5ba16-139">Under **All solutions**, find and choose hello tile for hello **Logic Apps Management** solution.</span></span>

   ![Zvolte "Správa aplikace logiky"](media/logic-apps-monitor-your-logic-apps-oms/logic-apps-management-tile2.png)

6. <span data-ttu-id="5ba16-141">Zvolte řešení hello tooinstall v vaším pracovním prostorem OMS **přidat**.</span><span class="sxs-lookup"><span data-stu-id="5ba16-141">tooinstall hello solution in your OMS workspace, choose **Add**.</span></span>

   ![Zvolte "Přidat" pro "Správa aplikace logiky"](media/logic-apps-monitor-your-logic-apps-oms/add-logic-apps-management-solution.png)

<a name="view-logic-app-runs-oms"></a>

## <a name="view-your-logic-app-runs-in-your-oms-workspace"></a><span data-ttu-id="5ba16-143">Zobrazení, které běží vaše aplikace logiky v pracovním prostoru OMS</span><span class="sxs-lookup"><span data-stu-id="5ba16-143">View your logic app runs in your OMS workspace</span></span>

1. <span data-ttu-id="5ba16-144">počet hello tooview a stav pro svou aplikaci logiky spustí, přejděte toohello přehledová stránka pracovního prostoru OMS.</span><span class="sxs-lookup"><span data-stu-id="5ba16-144">tooview hello count and status for your logic app runs, go toohello overview page for your OMS workspace.</span></span> <span data-ttu-id="5ba16-145">Zkontrolujte podrobnosti hello na hello **správy Logic Apps** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="5ba16-145">Review hello details on hello **Logic Apps Management** tile.</span></span>

   ![Dlaždice přehledu se zobrazuje počet logiku aplikace spustit a stav](media/logic-apps-monitor-your-logic-apps-oms/overview.png)

   > [!Note]
   > <span data-ttu-id="5ba16-147">Pokud místo hello správy Logic Apps dlaždice se zobrazí banner upgradu, zvolte hello banner tak, aby nejprve upgradovat vaším pracovním prostorem OMS.</span><span class="sxs-lookup"><span data-stu-id="5ba16-147">If this upgrade banner appears instead of hello Logic Apps Management tile, choose hello banner so that you upgrade your OMS workspace first.</span></span>
  
   > ![Upgrade "Pracovním prostorem OMS"](media/logic-apps-monitor-your-logic-apps-oms/oms-upgrade-banner.png)

2. <span data-ttu-id="5ba16-149">tooview souhrn s dalšími podrobnostmi o že logiku aplikace běží, zvolte hello **správy Logic Apps** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="5ba16-149">tooview a summary with more details about your logic app runs, choose hello **Logic Apps Management** tile.</span></span>

   <span data-ttu-id="5ba16-150">Zde že logiku aplikace běží jsou seskupené podle názvu nebo stav spuštění.</span><span class="sxs-lookup"><span data-stu-id="5ba16-150">Here, your logic app runs are grouped by name or by execution status.</span></span>

   ![Souhrn stavu pro svou aplikaci logiky běží](media/logic-apps-monitor-your-logic-apps-oms/logic-apps-runs-summary.png)
   
3. <span data-ttu-id="5ba16-152">tooview, které všechny hello se spouští pro určitou logiku aplikace nebo stav, vyberte hello řádek pro aplikace logiky nebo stavu.</span><span class="sxs-lookup"><span data-stu-id="5ba16-152">tooview all hello runs for a specific logic app or status, select hello row for a logic app or a status.</span></span>

   <span data-ttu-id="5ba16-153">Tady je příklad, který zobrazuje všechny hello spuštění pro určitou logiku aplikace:</span><span class="sxs-lookup"><span data-stu-id="5ba16-153">Here is an example that shows all hello runs for a specific logic app:</span></span>

   ![Spuštění zobrazení pro aplikaci logiky nebo ve stavu](media/logic-apps-monitor-your-logic-apps-oms/logic-app-run-details.png)

   > [!NOTE]
   > <span data-ttu-id="5ba16-155">Hello **opakované odeslání** sloupci se zobrazuje "Ano" pro spuštění, které jsou výsledkem znovu přijala spustit.</span><span class="sxs-lookup"><span data-stu-id="5ba16-155">hello **Resubmission** column shows "Yes" for runs that result from a resubmitted run.</span></span>

4. <span data-ttu-id="5ba16-156">toofilter těchto výsledků, můžete provést, klientské a serverové filtrování.</span><span class="sxs-lookup"><span data-stu-id="5ba16-156">toofilter these results, you can perform both client-side and server-side filtering.</span></span>

   * <span data-ttu-id="5ba16-157">Filtr na straně klienta: pro každý sloupec zvolte hello filtry, které chcete.</span><span class="sxs-lookup"><span data-stu-id="5ba16-157">Client-side filter: For each column, choose hello filters that you want.</span></span> 
   <span data-ttu-id="5ba16-158">Zde je několik příkladů:</span><span class="sxs-lookup"><span data-stu-id="5ba16-158">Here are some examples:</span></span>

     ![Příklad filtry sloupců](media/logic-apps-monitor-your-logic-apps-oms/filters.png)

   * <span data-ttu-id="5ba16-160">Filtr na straně serveru: toochoose určité časové období nebo toolimit hello několik spustí, které se zobrazují, použijte ovládací prvek oboru hello hello horní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="5ba16-160">Server-side filter: toochoose a specific time window or toolimit hello number of runs that appear, use hello scope control at hello top of hello page.</span></span> 
   <span data-ttu-id="5ba16-161">Ve výchozím nastavení zobrazí pouze 1000 záznamů najednou.</span><span class="sxs-lookup"><span data-stu-id="5ba16-161">By default, only 1,000 records appear at a time.</span></span> 
   
     ![Změna hello časový interval](media/logic-apps-monitor-your-logic-apps-oms/change-interval.png)
 
5. <span data-ttu-id="5ba16-163">všechny tooview hello akcí a jejich podrobnosti o konkrétní spustit, vyberte řádek, otevře se stránka hledání protokolů hello.</span><span class="sxs-lookup"><span data-stu-id="5ba16-163">tooview all hello actions and their details for a specific run, select a row, which opens hello Log Search page.</span></span> 

   * <span data-ttu-id="5ba16-164">Vyberte tyto informace v tabulce tooview **tabulky**.</span><span class="sxs-lookup"><span data-stu-id="5ba16-164">tooview this information in a table, choose **Table**.</span></span>
   * <span data-ttu-id="5ba16-165">dotaz hello toochange, můžete upravit řetězec dotazu hello v panelu vyhledávání hello.</span><span class="sxs-lookup"><span data-stu-id="5ba16-165">toochange hello query, you can edit hello query string in hello search bar.</span></span> 
   <span data-ttu-id="5ba16-166">Pro lepší podmínky, zvolte **pokročilé analýzy**.</span><span class="sxs-lookup"><span data-stu-id="5ba16-166">For a better experience, choose **Advanced Analytics**.</span></span>

     ![Zobrazit akce a podrobnosti pro spuštění aplikace logiky](media/logic-apps-monitor-your-logic-apps-oms/log-search-page.png)

     <span data-ttu-id="5ba16-168">V tomto poli na stránce hello analýzy protokolů Azure, můžete aktualizovat dotazů a zobrazení hello výsledky z tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="5ba16-168">Here on hello Azure Log Analytics page, you can update queries and view hello results from hello table.</span></span> 
     <span data-ttu-id="5ba16-169">Tento dotaz používá [Kusto dotazovací jazyk](https://docs.loganalytics.io/learn/tutorials/getting_started_with_queries.html), který můžete upravit, pokud chcete, aby tooview odlišné výsledky.</span><span class="sxs-lookup"><span data-stu-id="5ba16-169">This query uses [Kusto query language](https://docs.loganalytics.io/learn/tutorials/getting_started_with_queries.html), which you can edit if you want tooview different results.</span></span> 

     ![Azure Log Analytics - zobrazení dotazu](media/logic-apps-monitor-your-logic-apps-oms/query.png)

## <a name="next-steps"></a><span data-ttu-id="5ba16-171">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5ba16-171">Next steps</span></span>

* [<span data-ttu-id="5ba16-172">Monitorování zpráv B2B</span><span class="sxs-lookup"><span data-stu-id="5ba16-172">Monitor B2B messages</span></span>](../logic-apps/logic-apps-monitor-b2b-message.md)
