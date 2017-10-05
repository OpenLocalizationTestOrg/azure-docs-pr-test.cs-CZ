---
title: "Postup konfigurace dat vstupů pro úlohy Stream Analytics | Microsoft Docs"
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
ms.openlocfilehash: 1ffa517469da1a8d79917b9747abc97ca3bef463
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-configure-data-outputs-for-stream-analytics-jobs"></a><span data-ttu-id="1fffe-104">Postup konfigurace dat vstupů pro úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="1fffe-104">How to configure data outputs for Stream Analytics jobs</span></span>

<span data-ttu-id="1fffe-105">Úlohy Azure Stream Analytics může být připojen k jeden nebo více výstupů dat, které definuje připojení a jímky existující data.</span><span class="sxs-lookup"><span data-stu-id="1fffe-105">Azure Stream Analytics jobs can be connected to one or more data outputs, which define a connection to an existing data sink.</span></span> <span data-ttu-id="1fffe-106">Úlohu Stream Analytics procesy a transformuje příchozích dat, proud dat výstupních událostech je zapsán do výstupu dané úlohy.</span><span class="sxs-lookup"><span data-stu-id="1fffe-106">As your Stream Analytics job processes and transforms incoming data, a stream of data output events is written to your job's output.</span></span>

<span data-ttu-id="1fffe-107">Datový proud analytická data výstupy lze použít jako zdroj řídicí panely v reálném čase nebo výstrahy, spusťte pracovní postupy přesun dat nebo jednoduše archivaci dat pro pozdější zpracování dávky.</span><span class="sxs-lookup"><span data-stu-id="1fffe-107">Stream Analytics data outputs can be used to source real-time dashboards or alerts, trigger data movement workflows, or simply archive data for batch processing later on.</span></span> <span data-ttu-id="1fffe-108">Stream Analytics obsahuje prvotřídní integrace s několik služeb Azure, které jsou popsány zde podrobně.</span><span class="sxs-lookup"><span data-stu-id="1fffe-108">Stream Analytics has first class integration with several Azure services, which are documented in detail here.</span></span>

<span data-ttu-id="1fffe-109">Přidání výstup do úlohy Stream Analytics:</span><span class="sxs-lookup"><span data-stu-id="1fffe-109">To add an output to your Stream Analytics job:</span></span>

1. <span data-ttu-id="1fffe-110">V [portál Azure](https://portal.azure.com), spusťte úlohu a klikněte na tlačítko **výstupy** a pak klikněte na **přidat** v zobrazeném okně výstupy.</span><span class="sxs-lookup"><span data-stu-id="1fffe-110">In the [Azure portal](https://portal.azure.com), open your job and click **Outputs** and then click **Add** in the Outputs blade that appears.</span></span>
   
    ![Přidejte výstupy](./media/stream-analytics-add-outputs/1-stream-analytics-add-outputs.png)  
   
2. <span data-ttu-id="1fffe-112">Zadejte popisný název pro tento výstup v **Alias pro výstup** pole.</span><span class="sxs-lookup"><span data-stu-id="1fffe-112">Provide a friendly name for this output in the **Output Alias** box.</span></span> <span data-ttu-id="1fffe-113">Tento název můžete použít v dotazu vaše úlohy později na odkazovat na výstup.</span><span class="sxs-lookup"><span data-stu-id="1fffe-113">This name can be used in your job's query later on to refer to the output.</span></span>  
   
    <span data-ttu-id="1fffe-114">Vyplňte zbytek vlastnosti požadované připojení pro připojení k výstupu.</span><span class="sxs-lookup"><span data-stu-id="1fffe-114">Fill in the rest of the required connection properties to connect to your output.</span></span>  <span data-ttu-id="1fffe-115">Tato pole se liší podle typu výstupu a jsou definovány zde podrobně.</span><span class="sxs-lookup"><span data-stu-id="1fffe-115">These fields vary by output type and are defined in detail here.</span></span>  
   
    ![Výběr typu přesunu dat](./media/stream-analytics-add-outputs/2-stream-analytics-add-outputs.png)  
   
3. <span data-ttu-id="1fffe-117">V závislosti na typu výstupu musíte určit, jak je data serializovat nebo formátu.</span><span class="sxs-lookup"><span data-stu-id="1fffe-117">Depending on the output type, you may need to specify how the data is serialized or formatted.</span></span> <span data-ttu-id="1fffe-118">Nastavení konkrétní serializace pro každý typ výstupu jsou popsané v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="1fffe-118">The specific serialization settings for each output type are documented here.</span></span>
   
    <span data-ttu-id="1fffe-119">Vyplňte zbytek vlastnosti požadované připojení pro připojení ke zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="1fffe-119">Fill in the rest of the required connection properties to connect to your data source.</span></span> <span data-ttu-id="1fffe-120">Tato pole se liší podle typu vstupu a zdroj typu a jsou definovány v podrobně [vytvoření úlohy článku](stream-analytics-create-a-job.md).</span><span class="sxs-lookup"><span data-stu-id="1fffe-120">These fields vary by type of input and source type and are defined in detail in the [Create Job article](stream-analytics-create-a-job.md).</span></span>  

> [!Note]
>
> <span data-ttu-id="1fffe-121">Všechny element output přidány do úlohy, musí existovat před zahájení úlohy a události spuštěním toku.</span><span class="sxs-lookup"><span data-stu-id="1fffe-121">Any output element added to the job, must exist before the job is started and events start flowing.</span></span> <span data-ttu-id="1fffe-122">Například pokud použijete jako výstupu úložiště objektů Blob, úloha nebude vytvořit účet úložiště automaticky.</span><span class="sxs-lookup"><span data-stu-id="1fffe-122">For example, if you use Blob storage as an output, the job will not create a storage account automatically.</span></span> <span data-ttu-id="1fffe-123">Musí být před spuštěním úlohy ASA vytvořený uživatelem.</span><span class="sxs-lookup"><span data-stu-id="1fffe-123">It needs to be created by the user before the ASA job is started.</span></span>
> 
 

## <a name="get-help"></a><span data-ttu-id="1fffe-124">Podpora</span><span class="sxs-lookup"><span data-stu-id="1fffe-124">Get help</span></span>
<span data-ttu-id="1fffe-125">Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="1fffe-125">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="1fffe-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1fffe-126">Next steps</span></span>
* [<span data-ttu-id="1fffe-127">Úvod do služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="1fffe-127">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="1fffe-128">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="1fffe-128">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="1fffe-129">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="1fffe-129">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="1fffe-130">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="1fffe-130">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="1fffe-131">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="1fffe-131">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

