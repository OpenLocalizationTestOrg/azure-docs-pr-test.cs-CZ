---
title: "Postup vytvoření úlohy zpracování dat analytics pro Stream Analytics | Microsoft Docs"
description: "Vytvoření úlohy zpracování dat analytics pro Stream Analytics | učení segmentu cesty."
keywords: "zpracování analýzy dat"
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: e825fbcf-69e9-443f-b402-3b7a4568f415
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 05fdf1e20efd129cdfc27e1d37bc9e124edf5dcd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-create-a-data-analytics-processing-job-for-stream-analytics"></a><span data-ttu-id="e62d8-104">Postup vytvoření úlohy zpracování dat analytics pro Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="e62d8-104">How to create a data analytics processing job for Stream Analytics</span></span>
<span data-ttu-id="e62d8-105">Nejvyšší úrovně prostředků v Azure Stream Analytics je úlohy Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="e62d8-105">The top-level resource in Azure Stream Analytics is a Stream Analytics Job.</span></span>  <span data-ttu-id="e62d8-106">Obsahuje jeden nebo více zdrojů vstupních dat, dotaz vyjadřující transformaci dat a jeden nebo více cílů výstup, které výsledky se zapisují do.</span><span class="sxs-lookup"><span data-stu-id="e62d8-106">It consists of one or more input data sources, a query expressing the data transformation, and one or more output targets that results are written to.</span></span> <span data-ttu-id="e62d8-107">Společně tyto umožní uživatelům provádět analýzy dat, zpracování pro streamování dat scénáře.</span><span class="sxs-lookup"><span data-stu-id="e62d8-107">Together these enable the user to perform data analytics processing for streaming data scenarios.</span></span>

<span data-ttu-id="e62d8-108">Chcete-li začít používat Stream Analytics, začněte tím, že vytváření nové úlohy Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="e62d8-108">To start using Stream Analytics, begin by creating a new Stream Analytics job.</span></span>  <span data-ttu-id="e62d8-109">Všimněte si, že tato akce nemá žádné fakturace dopady až po zahájení úlohy.</span><span class="sxs-lookup"><span data-stu-id="e62d8-109">Note this action has no billing implications until the job is started.</span></span>

1. <span data-ttu-id="e62d8-110">Přihlaste se na online [portál Azure classic](http://manage.windowsazure.com) nebo [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="e62d8-110">Sign in on the online [Azure classic portal](http://manage.windowsazure.com) or the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="e62d8-111">Na portálu: **klikněte na tlačítko Nový**, pak klikněte na tlačítko **datové služby** nebo **Data Analytics** v závislosti na portál a pak klikněte na tlačítko **Azure Stream Analytics**nebo **Stream Analytics** a potom **rychle vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e62d8-111">In the portal: **Click New**, then click **Data Services** or **Data Analytics** depending on your portal and then click **Azure Stream Analytics** or **Stream Analytics** and then **Quick Create**.</span></span>
   
   ![Průvodce úlohy zpracování analýzy dat](./media/stream-analytics-create-a-job/1-stream-analytics-create-a-job.png)  
   
   ![Vytvoření analýzy dat, zpracování úlohy](./media/stream-analytics-create-a-job/4-stream-analytics-create-a-job.png)  
3. <span data-ttu-id="e62d8-114">Zadejte požadovanou konfiguraci pro úlohu služby Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="e62d8-114">Specify the desired configuration for the Stream Analytics job.</span></span>
   
   * <span data-ttu-id="e62d8-115">V **název úlohy** pole, zadejte název pro identifikaci úlohu služby Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="e62d8-115">In the **Job Name** box, enter a name to identify the Stream Analytics job.</span></span> <span data-ttu-id="e62d8-116">Když **název úlohy** byl ověřen, do pole Název úlohy se zobrazí zelená značka zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="e62d8-116">When the **Job Name** is validated, a green check mark appears in the Job Name box.</span></span> <span data-ttu-id="e62d8-117">**Název úlohy** může obsahovat pouze alfanumerické znaky a '-' znak a musí být v rozmezí 3 až 63 znaků.</span><span class="sxs-lookup"><span data-stu-id="e62d8-117">The **Job Name** may contain only alphanumeric characters and the '-' character, and must be between 3 and 63 characters.</span></span>
   * <span data-ttu-id="e62d8-118">Použití **oblast** na portálu Azure nebo **umístění** na portálu Azure k určení zeměpisného umístění, ve které chcete spustit úlohu.</span><span class="sxs-lookup"><span data-stu-id="e62d8-118">Use **Region** in the Azure portal or **Location** in the Azure portal to specify the geographic location where you want to run the job.</span></span>
   * <span data-ttu-id="e62d8-119">Pokud používáte portál Azure, vyberte nebo vytvořte účet úložiště, který chcete použít jako **místní monitorování účtu úložiště**.</span><span class="sxs-lookup"><span data-stu-id="e62d8-119">If using the Azure portal, select or create a storage account to use as the **Regional Monitoring Storage Account**.</span></span> <span data-ttu-id="e62d8-120">Tento účet úložiště se používá k ukládání dat monitorování pro všechny úlohy služby Stream Analytics, které jsou spuštěné v této oblasti.</span><span class="sxs-lookup"><span data-stu-id="e62d8-120">This storage account is used to store monitoring data for all Stream Analytics jobs running in this region.</span></span>
   * <span data-ttu-id="e62d8-121">Pokud používáte portál Azure, zadejte nový nebo existující **skupiny prostředků** pro uložení související prostředky pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e62d8-121">If using the Azure portal, specify a new or existing **Resource Group** to hold related resources for your application.</span></span>
4. <span data-ttu-id="e62d8-122">Po nakonfigurování nastavení nové úlohy Stream Analytics, klikněte na tlačítko **vytvořit úlohy Stream Analytics**.</span><span class="sxs-lookup"><span data-stu-id="e62d8-122">Once the new Stream Analytics job options are configured, click **Create Stream Analytics Job**.</span></span> <span data-ttu-id="e62d8-123">Může trvat několik minut pro úlohu služby Stream Analytics, který se má vytvořit.</span><span class="sxs-lookup"><span data-stu-id="e62d8-123">It can take a few minutes for the Stream Analytics job to be created.</span></span> <span data-ttu-id="e62d8-124">Chcete-li zkontrolovat stav, můžete sledovat průběh v centru oznámení.</span><span class="sxs-lookup"><span data-stu-id="e62d8-124">To check the status, you can monitor the progress in the Notifications hub.</span></span>
   
   ![Analýza dat, zpracování centrem oznámení úlohy](./media/stream-analytics-create-a-job/2-stream-analytics-create-a-job.png)  
   
   ![Vytvoření úlohy úlohy Azure portálu Data analytics zpracování](./media/stream-analytics-create-a-job/5-stream-analytics-create-a-job.png)  
5. <span data-ttu-id="e62d8-127">Nová úloha se zobrazí stav **vytvořená**.</span><span class="sxs-lookup"><span data-stu-id="e62d8-127">The new job will show a status of **Created**.</span></span> <span data-ttu-id="e62d8-128">Všimněte si, že **spustit** tlačítko k dispozici.</span><span class="sxs-lookup"><span data-stu-id="e62d8-128">Notice that the **Start** button is disabled.</span></span> <span data-ttu-id="e62d8-129">Před spuštěním úlohy, nakonfigurujte vstupu úlohy, dotaz a výstup.</span><span class="sxs-lookup"><span data-stu-id="e62d8-129">Configure the job input, query, and output before you start the job.</span></span>
   
   ![Úlohy zpracování analýzy dat stavu](./media/stream-analytics-create-a-job/3-stream-analytics-create-a-job.png)  
   
   ![Azure portálu analýzy dat zpracování stavu úlohy](./media/stream-analytics-create-a-job/6-stream-analytics-create-a-job.png)  

## <a name="get-help"></a><span data-ttu-id="e62d8-132">Podpora</span><span class="sxs-lookup"><span data-stu-id="e62d8-132">Get help</span></span>
<span data-ttu-id="e62d8-133">Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="e62d8-133">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="e62d8-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e62d8-134">Next steps</span></span>
* [<span data-ttu-id="e62d8-135">Úvod do služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="e62d8-135">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="e62d8-136">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="e62d8-136">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="e62d8-137">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="e62d8-137">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="e62d8-138">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="e62d8-138">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="e62d8-139">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="e62d8-139">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

