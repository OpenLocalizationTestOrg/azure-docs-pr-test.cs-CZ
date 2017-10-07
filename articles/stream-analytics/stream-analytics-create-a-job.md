---
title: "aaaHow toocreate zpracování úlohy analýzy datového pro Stream Analytics | Microsoft Docs"
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
ms.openlocfilehash: d4a3c89d8862d59688d06a1719b063efa2ab1c93
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-data-analytics-processing-job-for-stream-analytics"></a><span data-ttu-id="a01f6-104">Jak toocreate zpracování analýzy data úlohy pro Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="a01f6-104">How toocreate a data analytics processing job for Stream Analytics</span></span>
<span data-ttu-id="a01f6-105">Hello nejvyšší úrovně prostředků v Azure Stream Analytics je úlohy Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="a01f6-105">hello top-level resource in Azure Stream Analytics is a Stream Analytics Job.</span></span>  <span data-ttu-id="a01f6-106">Se skládá z jednoho nebo více vstupních zdrojů dat, dotaz vyjadřující hello transformaci dat a jeden nebo více cílů výstup, zapsaných do výsledků.</span><span class="sxs-lookup"><span data-stu-id="a01f6-106">It consists of one or more input data sources, a query expressing hello data transformation, and one or more output targets that results are written to.</span></span> <span data-ttu-id="a01f6-107">Společně tyto povolit hello tooperform data analýzy chování uživatelů zpracování dat scénáře.</span><span class="sxs-lookup"><span data-stu-id="a01f6-107">Together these enable hello user tooperform data analytics processing for streaming data scenarios.</span></span>

<span data-ttu-id="a01f6-108">toostart pomocí Stream Analytics, začněte tím, že vytváření nové úlohy Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="a01f6-108">toostart using Stream Analytics, begin by creating a new Stream Analytics job.</span></span>  <span data-ttu-id="a01f6-109">Všimněte si, že tato akce nemá žádné fakturace dopady až po zahájení úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="a01f6-109">Note this action has no billing implications until hello job is started.</span></span>

1. <span data-ttu-id="a01f6-110">Přihlaste se na hello online [portál Azure classic](http://manage.windowsazure.com) nebo hello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="a01f6-110">Sign in on hello online [Azure classic portal](http://manage.windowsazure.com) or hello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="a01f6-111">Portálu hello: **klikněte na tlačítko Nový**, pak klikněte na tlačítko **datové služby** nebo **Data Analytics** v závislosti na portál a pak klikněte na tlačítko **Azure Stream Analytics** nebo **Stream Analytics** a potom **rychle vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a01f6-111">In hello portal: **Click New**, then click **Data Services** or **Data Analytics** depending on your portal and then click **Azure Stream Analytics** or **Stream Analytics** and then **Quick Create**.</span></span>
   
   ![Průvodce úlohy zpracování analýzy dat](./media/stream-analytics-create-a-job/1-stream-analytics-create-a-job.png)  
   
   ![Vytvoření analýzy dat, zpracování úlohy](./media/stream-analytics-create-a-job/4-stream-analytics-create-a-job.png)  
3. <span data-ttu-id="a01f6-114">Zadejte požadovanou konfiguraci úlohy služby Stream Analytics hello hello.</span><span class="sxs-lookup"><span data-stu-id="a01f6-114">Specify hello desired configuration for hello Stream Analytics job.</span></span>
   
   * <span data-ttu-id="a01f6-115">V hello **název úlohy** zadejte úloha Stream Analytics název tooidentify hello.</span><span class="sxs-lookup"><span data-stu-id="a01f6-115">In hello **Job Name** box, enter a name tooidentify hello Stream Analytics job.</span></span> <span data-ttu-id="a01f6-116">Když hello **název úlohy** byl ověřen, v poli Název úlohy hello se zobrazí zelená značka zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="a01f6-116">When hello **Job Name** is validated, a green check mark appears in hello Job Name box.</span></span> <span data-ttu-id="a01f6-117">Hello **název úlohy** může obsahovat pouze alfanumerické znaky a hello '-' znak a musí být v rozmezí 3 až 63 znaků.</span><span class="sxs-lookup"><span data-stu-id="a01f6-117">hello **Job Name** may contain only alphanumeric characters and hello '-' character, and must be between 3 and 63 characters.</span></span>
   * <span data-ttu-id="a01f6-118">Použití **oblast** v hello portál Azure nebo **umístění** v hello Azure portálu toospecify hello zeměpisného umístění, kam chcete toorun hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="a01f6-118">Use **Region** in hello Azure portal or **Location** in hello Azure portal toospecify hello geographic location where you want toorun hello job.</span></span>
   * <span data-ttu-id="a01f6-119">Pokud pomocí hello portálu Azure, vyberte nebo vytvořte toouse účtu úložiště jako hello **místní monitorování účtu úložiště**.</span><span class="sxs-lookup"><span data-stu-id="a01f6-119">If using hello Azure portal, select or create a storage account toouse as hello **Regional Monitoring Storage Account**.</span></span> <span data-ttu-id="a01f6-120">Tento účet úložiště je použité toostore monitorování data pro všechny úlohy služby Stream Analytics, které jsou spuštěné v této oblasti.</span><span class="sxs-lookup"><span data-stu-id="a01f6-120">This storage account is used toostore monitoring data for all Stream Analytics jobs running in this region.</span></span>
   * <span data-ttu-id="a01f6-121">Pokud pomocí hello portálu Azure, zadejte nový nebo existující **skupiny prostředků** toohold související prostředky pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a01f6-121">If using hello Azure portal, specify a new or existing **Resource Group** toohold related resources for your application.</span></span>
4. <span data-ttu-id="a01f6-122">Po nakonfigurování hello nové možnosti úlohy Stream Analytics, klikněte na tlačítko **vytvořit úlohy Stream Analytics**.</span><span class="sxs-lookup"><span data-stu-id="a01f6-122">Once hello new Stream Analytics job options are configured, click **Create Stream Analytics Job**.</span></span> <span data-ttu-id="a01f6-123">Může trvat několik minut, než toobe úlohy Stream Analytics hello vytvořili.</span><span class="sxs-lookup"><span data-stu-id="a01f6-123">It can take a few minutes for hello Stream Analytics job toobe created.</span></span> <span data-ttu-id="a01f6-124">toocheck hello stav, můžete sledovat průběh hello v centru oznámení hello.</span><span class="sxs-lookup"><span data-stu-id="a01f6-124">toocheck hello status, you can monitor hello progress in hello Notifications hub.</span></span>
   
   ![Analýza dat, zpracování centrem oznámení úlohy](./media/stream-analytics-create-a-job/2-stream-analytics-create-a-job.png)  
   
   ![Vytvoření úlohy úlohy Azure portálu Data analytics zpracování](./media/stream-analytics-create-a-job/5-stream-analytics-create-a-job.png)  
5. <span data-ttu-id="a01f6-127">Nová úloha Hello se zobrazí stav **vytvořená**.</span><span class="sxs-lookup"><span data-stu-id="a01f6-127">hello new job will show a status of **Created**.</span></span> <span data-ttu-id="a01f6-128">Všimněte si, že hello **spustit** tlačítko k dispozici.</span><span class="sxs-lookup"><span data-stu-id="a01f6-128">Notice that hello **Start** button is disabled.</span></span> <span data-ttu-id="a01f6-129">Před zahájením hello úlohy konfigurace hello úlohy vstup, dotaz a výstup.</span><span class="sxs-lookup"><span data-stu-id="a01f6-129">Configure hello job input, query, and output before you start hello job.</span></span>
   
   ![Úlohy zpracování analýzy dat stavu](./media/stream-analytics-create-a-job/3-stream-analytics-create-a-job.png)  
   
   ![Azure portálu analýzy dat zpracování stavu úlohy](./media/stream-analytics-create-a-job/6-stream-analytics-create-a-job.png)  

## <a name="get-help"></a><span data-ttu-id="a01f6-132">Podpora</span><span class="sxs-lookup"><span data-stu-id="a01f6-132">Get help</span></span>
<span data-ttu-id="a01f6-133">Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="a01f6-133">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="a01f6-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a01f6-134">Next steps</span></span>
* [<span data-ttu-id="a01f6-135">Úvod tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="a01f6-135">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="a01f6-136">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="a01f6-136">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="a01f6-137">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="a01f6-137">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="a01f6-138">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="a01f6-138">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="a01f6-139">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="a01f6-139">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

