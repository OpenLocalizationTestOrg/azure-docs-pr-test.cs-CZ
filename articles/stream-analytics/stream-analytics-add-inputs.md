---
title: "Přidat vstup data pro své úlohy Stream Analytics | Microsoft Docs"
description: "Zjistěte, jak spojit zdroje dat do vaší úlohy Stream Analytics jako vysílání datového proudu vstupní data ze služby Event Hubs nebo odkaz na data z úložiště blogu."
keywords: "data vstup, streamování dat"
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: 
ms.assetid: 9e59bd24-2a80-4ecb-b6b2-309a07c70bcd
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 8bdbcf78f2892cbd1e1cc09cef220dff08dd9490
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="add-a-streaming-data-input-or-reference-data-to-a-stream-analytics-job"></a><span data-ttu-id="52512-104">Přidat streamování data vstup nebo referenční data do úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="52512-104">Add a streaming data input or reference data to a Stream Analytics job</span></span>
<span data-ttu-id="52512-105">Zjistěte, jak spojit zdroje dat do vaší úlohy Stream Analytics jako vysílání datového proudu vstupní data ze služby Event Hubs nebo odkaz na data z úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="52512-105">Learn how to hook up a data source to your Stream Analytics job as streaming data input from Event Hubs or reference data from Blob storage.</span></span>

<span data-ttu-id="52512-106">Úlohy Azure Stream Analytics může být připojen k jeden datový vstup nebo informace, z nichž každý definuje připojení a stávajícího zdroje dat.</span><span class="sxs-lookup"><span data-stu-id="52512-106">Azure Stream Analytics jobs can be connected to one data input or more, each of which define a connection to an existing data source.</span></span> <span data-ttu-id="52512-107">Protože data se odešlou do zdroje dat, je spotřebovávají úlohu služby Stream Analytics a zpracování v reálném čase jako datový proud.</span><span class="sxs-lookup"><span data-stu-id="52512-107">As data is sent to that data source, it is consumed by the Stream Analytics job and processed in real time as streaming data.</span></span> <span data-ttu-id="52512-108">Stream Analytics obsahuje prvotřídní integrace s [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) a [úložiště objektů Azure Blob](../storage/blobs/storage-dotnet-how-to-use-blobs.md) v rámci i mimo ni úlohy předplatného.</span><span class="sxs-lookup"><span data-stu-id="52512-108">Stream Analytics has first class integration with [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) and [Azure Blob storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md) both within and outside of the job's subscription.</span></span>

<span data-ttu-id="52512-109">Tento článek je krok v [Stream Analytics studijní](/documentation/learning-paths/stream-analytics/).</span><span class="sxs-lookup"><span data-stu-id="52512-109">This article is a step in the [Stream Analytics learning path](/documentation/learning-paths/stream-analytics/).</span></span>

## <a name="data-input-streaming-data-and-reference-data"></a><span data-ttu-id="52512-110">Vstupní data: streamování dat a referenčních dat</span><span class="sxs-lookup"><span data-stu-id="52512-110">Data input: Streaming data and reference data</span></span>
<span data-ttu-id="52512-111">Existují dva odlišné typy vstupů ve Stream Analytics: datové proudy a referenční data.</span><span class="sxs-lookup"><span data-stu-id="52512-111">There are two distinct types of inputs in Stream Analytics: data streams and reference data.</span></span>

* <span data-ttu-id="52512-112">**Datové proudy**: úlohy Stream Analytics musí obsahovat alespoň jeden vstup datového streamu spotřebované a transformovat úlohou.</span><span class="sxs-lookup"><span data-stu-id="52512-112">**Data Streams**: Stream Analytics jobs must include at least one data stream input to be consumed and transformed by the job.</span></span> <span data-ttu-id="52512-113">Azure Blob storage a Azure Event Hubs se podporují jako vstupní zdroje dat datového proudu.</span><span class="sxs-lookup"><span data-stu-id="52512-113">Azure Blob storage and Azure Event Hubs are supported as data stream input sources.</span></span> <span data-ttu-id="52512-114">Azure Event Hubs slouží ke shromažďování streamů událostí z připojených zařízení, služeb a aplikací.</span><span class="sxs-lookup"><span data-stu-id="52512-114">Azure Event Hubs are used to collect event streams from connected devices, services and applications.</span></span> <span data-ttu-id="52512-115">Azure Blob storage můžete použít jako vstupní zdroj pro příjem dat hromadné jako datový proud.</span><span class="sxs-lookup"><span data-stu-id="52512-115">Azure Blob storage can be used as an input source for ingesting bulk data as a stream.</span></span>  
* <span data-ttu-id="52512-116">**Referenční data**: Stream Analytics podporuje druhého typu pomocného vstupní volané referenční data.</span><span class="sxs-lookup"><span data-stu-id="52512-116">**Reference data**: Stream Analytics supports a second type of auxiliary input called reference data.</span></span>  <span data-ttu-id="52512-117">Oproti dat přesouvaných tato data jsou statická nebo zpomalení změna.</span><span class="sxs-lookup"><span data-stu-id="52512-117">As opposed to data in motion, this data is static or slowing changing.</span></span>  <span data-ttu-id="52512-118">Obvykle se používá pro provádění look-ups a korelací s datové proudy vytvořit bohatší datové sady.</span><span class="sxs-lookup"><span data-stu-id="52512-118">It is typically used for performing look-ups and correlations with data streams to create a richer data set.</span></span>  <span data-ttu-id="52512-119">Azure Blob storage je aktuálně podporované pouze vstupní zdroj pro referenční data.</span><span class="sxs-lookup"><span data-stu-id="52512-119">Azure Blob storage is currently the only supported input source for reference data.</span></span>  

<span data-ttu-id="52512-120">Chcete-li přidat vstup do úlohy Stream Analytics:</span><span class="sxs-lookup"><span data-stu-id="52512-120">To add an input to your Stream Analytics job:</span></span>

1. <span data-ttu-id="52512-121">V portálu Azure klikněte na **vstupy** a pak klikněte na **přidat vstup** v úloze Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="52512-121">In the Azure portal click **Inputs** and then click **Add an Input** in your Stream Analytics job.</span></span>
   
    ![Portál Azure classic – přidat vstup.](./media/stream-analytics-add-inputs/1-stream-analytics-add-inputs.png)  
   
    <span data-ttu-id="52512-123">V portálu Azure klikněte na **vstupy** dlaždici v úloze Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="52512-123">In the Azure portal click the **Inputs** tile in your Stream Analytics job.</span></span>  
   
    ![Portál Azure – přidání datového vstupu.](./media/stream-analytics-add-inputs/7-stream-analytics-add-inputs.png)  
2. <span data-ttu-id="52512-125">Zadejte typ vstupu: buď **datový proud** nebo **referenční data**.</span><span class="sxs-lookup"><span data-stu-id="52512-125">Specify the type of the input: either **Data stream** or **Reference data**.</span></span>
   
    ![Přidání správná data vstup, streamování nebo odkaz](./media/stream-analytics-add-inputs/2-stream-analytics-add-inputs.png)  
   
    ![Přidání správná data vstup, streamování nebo odkaz](./media/stream-analytics-add-inputs/8-stream-analytics-add-inputs.png)  
3. <span data-ttu-id="52512-128">Pokud vytváříte vstupní datový proud, zadejte typ zdroje pro vstup.</span><span class="sxs-lookup"><span data-stu-id="52512-128">If creating a Data Stream input, specify the source type for the input.</span></span>  <span data-ttu-id="52512-129">Během vytváření referenční Data jako jenom objekt Blob úložiště je podporována v tuto chvíli můžete tento krok přeskočen.</span><span class="sxs-lookup"><span data-stu-id="52512-129">This step can be skipped during Reference Data creation as only Blob storage is supported at this time.</span></span>
   
    ![Přidat vstup datového streamu dat](./media/stream-analytics-add-inputs/3-stream-analytics-add-inputs.png)  
   
    ![Přidat vstup datového streamu dat](./media/stream-analytics-add-inputs/9-stream-analytics-add-inputs.png)  
4. <span data-ttu-id="52512-132">Zadejte popisný název pro tento vstup do pole vstupní Alias.</span><span class="sxs-lookup"><span data-stu-id="52512-132">Provide a friendly name for this input in the Input Alias box.</span></span>  <span data-ttu-id="52512-133">Tento název se použije v dotazu vaše úlohy později na odkazovat na vstup.</span><span class="sxs-lookup"><span data-stu-id="52512-133">This name will be used in your job's query later on to refer to the input.</span></span>
   
    <span data-ttu-id="52512-134">Vyplňte zbytek vlastnosti požadované připojení pro připojení ke zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="52512-134">Fill in the rest of the required connection properties to connect to your data source.</span></span> <span data-ttu-id="52512-135">Tato pole se liší podle typu vstupu a zdroj typu a jsou definovány podrobně [zde](stream-analytics-create-a-job.md).</span><span class="sxs-lookup"><span data-stu-id="52512-135">These fields vary by type of input and source type and are defined in detail [here](stream-analytics-create-a-job.md).</span></span>  
   
    ![Přidat vstup datového centra událostí](./media/stream-analytics-add-inputs/4-stream-analytics-add-inputs.png)  
5. <span data-ttu-id="52512-137">Zadejte nastavení serializace pro vstupní data:</span><span class="sxs-lookup"><span data-stu-id="52512-137">Specify the serialization settings for the input data:</span></span>
   
   * <span data-ttu-id="52512-138">Abyste měli jistotu, dotazy fungovaly podle očekávání, zadejte **formát serializace událostí** příchozích dat.</span><span class="sxs-lookup"><span data-stu-id="52512-138">To make sure your queries work the way you expect, specify the **Event Serialization Format** of incoming data.</span></span>  <span data-ttu-id="52512-139">Formáty podporované serializace jsou JSON, CSV a Avro.</span><span class="sxs-lookup"><span data-stu-id="52512-139">Supported serialization formats are JSON, CSV, and Avro.</span></span>
   * <span data-ttu-id="52512-140">Ověřte **kódování** pro data.</span><span class="sxs-lookup"><span data-stu-id="52512-140">Verify the **Encoding** for the data.</span></span>  <span data-ttu-id="52512-141">V tuto chvíli je jediným podporovaným formátem kódování UTF-8.</span><span class="sxs-lookup"><span data-stu-id="52512-141">UTF-8 is the only supported encoding format at this time.</span></span>
     
     ![Nastavení serializace dat pro vložení dat](./media/stream-analytics-add-inputs/5-stream-analytics-add-inputs.png)  
     
     ![Nastavení serializace dat pro vložení dat](./media/stream-analytics-add-inputs/10-stream-analytics-add-inputs.png)  
6. <span data-ttu-id="52512-144">Po dokončení vytvoření vstupní, Stream Analytics ověří, že se můžete připojit k vstupního zdroje.</span><span class="sxs-lookup"><span data-stu-id="52512-144">After completing the input creation, Stream Analytics will verify that it can connect to the input source.</span></span>  <span data-ttu-id="52512-145">Stav operace testování připojení můžete zobrazit v centru oznámení.</span><span class="sxs-lookup"><span data-stu-id="52512-145">You can view the status of the Test Connection operation in the Notification hub.</span></span>
   
    ![Testovací připojení vstupních dat](./media/stream-analytics-add-inputs/6-stream-analytics-add-inputs.png)  
   
    ![Testovací připojení vstupních dat](./media/stream-analytics-add-inputs/11-stream-analytics-add-inputs.png)  

## <a name="get-help-with-streaming-data-inputs"></a><span data-ttu-id="52512-148">Získat pomoc s streamování vstupů dat.</span><span class="sxs-lookup"><span data-stu-id="52512-148">Get help with streaming data inputs</span></span>
<span data-ttu-id="52512-149">Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="52512-149">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="52512-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="52512-150">Next steps</span></span>
* [<span data-ttu-id="52512-151">Úvod do služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="52512-151">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="52512-152">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="52512-152">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="52512-153">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="52512-153">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="52512-154">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="52512-154">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="52512-155">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="52512-155">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

