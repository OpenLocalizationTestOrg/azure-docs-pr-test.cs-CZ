---
title: "aaaHow tooconfigure data vstupů pro úlohy Stream Analytics | Microsoft Docs"
description: "Nakonfigurujte výstupy pro úlohy Stream Analytics | učení segmentu cesty."
keywords: "data výstupu přesun dat"
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 3bbea3da-bfce-4af1-a15e-d4b23874034f
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/26/2017
ms.author: samacha
ms.openlocfilehash: c5d89e9e9f9211d3e778580c071dd53d56aed9fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-data-outputs-for-stream-analytics-jobs"></a><span data-ttu-id="e81b3-104">Jak se tooconfigure data vstupů pro úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="e81b3-104">How tooconfigure data outputs for Stream Analytics jobs</span></span>

<span data-ttu-id="e81b3-105">Úlohy Azure Stream Analytics může být připojené tooone nebo další výstupy dat, které definují existující data podřízený tooan připojení.</span><span class="sxs-lookup"><span data-stu-id="e81b3-105">Azure Stream Analytics jobs can be connected tooone or more data outputs, which define a connection tooan existing data sink.</span></span> <span data-ttu-id="e81b3-106">Úlohu Stream Analytics procesy a transformuje příchozích dat, proud událostí výstupní data je zapsán výstup tooyour úlohy.</span><span class="sxs-lookup"><span data-stu-id="e81b3-106">As your Stream Analytics job processes and transforms incoming data, a stream of data output events is written tooyour job's output.</span></span>

<span data-ttu-id="e81b3-107">Stream Analytics data výstupy může být v reálném čase řídicí panely použité toosource nebo výstrahy, pracovní postupy přesun dat aktivační události nebo jednoduše archivu data pro pozdější zpracování dávky.</span><span class="sxs-lookup"><span data-stu-id="e81b3-107">Stream Analytics data outputs can be used toosource real-time dashboards or alerts, trigger data movement workflows, or simply archive data for batch processing later on.</span></span> <span data-ttu-id="e81b3-108">Stream Analytics obsahuje prvotřídní integrace s několik služeb Azure, které jsou popsány zde podrobně.</span><span class="sxs-lookup"><span data-stu-id="e81b3-108">Stream Analytics has first class integration with several Azure services, which are documented in detail here.</span></span>

<span data-ttu-id="e81b3-109">tooadd úlohu služby Stream Analytics tooyour výstup:</span><span class="sxs-lookup"><span data-stu-id="e81b3-109">tooadd an output tooyour Stream Analytics job:</span></span>

1. <span data-ttu-id="e81b3-110">V hello [portál Azure](https://portal.azure.com), spusťte úlohu a klikněte na tlačítko **výstupy** a pak klikněte na **přidat** v zobrazeném okně výstupy hello.</span><span class="sxs-lookup"><span data-stu-id="e81b3-110">In hello [Azure portal](https://portal.azure.com), open your job and click **Outputs** and then click **Add** in hello Outputs blade that appears.</span></span>
   
    ![Přidejte výstupy](./media/stream-analytics-add-outputs/1-stream-analytics-add-outputs.png)  
   
2. <span data-ttu-id="e81b3-112">Zadejte popisný název pro tento výstup v hello **Alias pro výstup** pole.</span><span class="sxs-lookup"><span data-stu-id="e81b3-112">Provide a friendly name for this output in hello **Output Alias** box.</span></span> <span data-ttu-id="e81b3-113">Tento název dá vaše úloha dotaz později na toorefer toohello výstup.</span><span class="sxs-lookup"><span data-stu-id="e81b3-113">This name can be used in your job's query later on toorefer toohello output.</span></span>  
   
    <span data-ttu-id="e81b3-114">Vyplňte hello rest hello vyžaduje připojení vlastnosti tooconnect tooyour výstupu.</span><span class="sxs-lookup"><span data-stu-id="e81b3-114">Fill in hello rest of hello required connection properties tooconnect tooyour output.</span></span>  <span data-ttu-id="e81b3-115">Tato pole se liší podle typu výstupu a jsou definovány zde podrobně.</span><span class="sxs-lookup"><span data-stu-id="e81b3-115">These fields vary by output type and are defined in detail here.</span></span>  
   
    ![Výběr typu přesunu dat](./media/stream-analytics-add-outputs/2-stream-analytics-add-outputs.png)  
   
3. <span data-ttu-id="e81b3-117">V závislosti na typu výstup hello může být nutné toospecify jak hello data serializovaná nebo formátu.</span><span class="sxs-lookup"><span data-stu-id="e81b3-117">Depending on hello output type, you may need toospecify how hello data is serialized or formatted.</span></span> <span data-ttu-id="e81b3-118">nastavení Hello konkrétní serializace pro každý typ výstupu jsou popsané v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="e81b3-118">hello specific serialization settings for each output type are documented here.</span></span>
   
    <span data-ttu-id="e81b3-119">Vyplňte hello rest hello požadované připojení vlastnosti zdroje dat tooconnect tooyour.</span><span class="sxs-lookup"><span data-stu-id="e81b3-119">Fill in hello rest of hello required connection properties tooconnect tooyour data source.</span></span> <span data-ttu-id="e81b3-120">Tato pole se liší podle typu vstupu a zdroj typu a jsou definovány v podrobně hello [vytvoření úlohy článku](stream-analytics-create-a-job.md).</span><span class="sxs-lookup"><span data-stu-id="e81b3-120">These fields vary by type of input and source type and are defined in detail in hello [Create Job article](stream-analytics-create-a-job.md).</span></span>  

> [!Note]
>
> <span data-ttu-id="e81b3-121">Všechny úlohy přidané toohello výstup elementu musí existovat hello úloha se spustila a události spuštění toku.</span><span class="sxs-lookup"><span data-stu-id="e81b3-121">Any output element added toohello job, must exist before hello job is started and events start flowing.</span></span> <span data-ttu-id="e81b3-122">Například pokud použijete jako výstupu úložiště objektů Blob, úlohy hello nebude vytvořit účet úložiště automaticky.</span><span class="sxs-lookup"><span data-stu-id="e81b3-122">For example, if you use Blob storage as an output, hello job will not create a storage account automatically.</span></span> <span data-ttu-id="e81b3-123">Potřebuje toobe vytvořené uživatelem hello před zahájení úlohy ASA hello.</span><span class="sxs-lookup"><span data-stu-id="e81b3-123">It needs toobe created by hello user before hello ASA job is started.</span></span>
> 
 

## <a name="get-help"></a><span data-ttu-id="e81b3-124">Podpora</span><span class="sxs-lookup"><span data-stu-id="e81b3-124">Get help</span></span>
<span data-ttu-id="e81b3-125">Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="e81b3-125">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="e81b3-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e81b3-126">Next steps</span></span>
* [<span data-ttu-id="e81b3-127">Úvod tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="e81b3-127">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="e81b3-128">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="e81b3-128">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="e81b3-129">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="e81b3-129">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="e81b3-130">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="e81b3-130">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="e81b3-131">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="e81b3-131">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

