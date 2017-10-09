---
title: "aaaAdd dat vstupu úlohy Stream Analytics tooyour | Microsoft Docs"
description: "Zjistěte, jak toohook nahoru tooyour zdroje dat Stream Analytics úlohy jako streamování vstupní data ze služby Event Hubs nebo odkaz na data z úložiště blogu."
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
ms.openlocfilehash: 674000268fcdf9bc000af3e2f166cb66f1366922
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-streaming-data-input-or-reference-data-tooa-stream-analytics-job"></a><span data-ttu-id="617fa-104">Přidat streamování dat vstup nebo referenční data tooa úloha Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="617fa-104">Add a streaming data input or reference data tooa Stream Analytics job</span></span>
<span data-ttu-id="617fa-105">Zjistěte, jak toohook nahoru tooyour zdroje dat Stream Analytics úlohy jako streamování vstupní data ze služby Event Hubs nebo odkaz na data z úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="617fa-105">Learn how toohook up a data source tooyour Stream Analytics job as streaming data input from Event Hubs or reference data from Blob storage.</span></span>

<span data-ttu-id="617fa-106">Úlohy Azure Stream Analytics může být připojené tooone vstupní data nebo informace, z nichž každý definujte připojení tooan existující zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="617fa-106">Azure Stream Analytics jobs can be connected tooone data input or more, each of which define a connection tooan existing data source.</span></span> <span data-ttu-id="617fa-107">Protože data se odesílají toothat zdroj dat, je spotřebovávají úlohy služby Stream Analytics hello a zpracování v reálném čase jako datový proud.</span><span class="sxs-lookup"><span data-stu-id="617fa-107">As data is sent toothat data source, it is consumed by hello Stream Analytics job and processed in real time as streaming data.</span></span> <span data-ttu-id="617fa-108">Stream Analytics obsahuje prvotřídní integrace s [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) a [úložiště objektů Azure Blob](../storage/blobs/storage-dotnet-how-to-use-blobs.md) v rámci i mimo ni hello úlohu odběru.</span><span class="sxs-lookup"><span data-stu-id="617fa-108">Stream Analytics has first class integration with [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) and [Azure Blob storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md) both within and outside of hello job's subscription.</span></span>

<span data-ttu-id="617fa-109">Tento článek je krok v hello [Stream Analytics studijní](/documentation/learning-paths/stream-analytics/).</span><span class="sxs-lookup"><span data-stu-id="617fa-109">This article is a step in hello [Stream Analytics learning path](/documentation/learning-paths/stream-analytics/).</span></span>

## <a name="data-input-streaming-data-and-reference-data"></a><span data-ttu-id="617fa-110">Vstupní data: streamování dat a referenčních dat</span><span class="sxs-lookup"><span data-stu-id="617fa-110">Data input: Streaming data and reference data</span></span>
<span data-ttu-id="617fa-111">Existují dva odlišné typy vstupů ve Stream Analytics: datové proudy a referenční data.</span><span class="sxs-lookup"><span data-stu-id="617fa-111">There are two distinct types of inputs in Stream Analytics: data streams and reference data.</span></span>

* <span data-ttu-id="617fa-112">**Datové proudy**: úlohy Stream Analytics musí obsahovat alespoň jeden data datového proudu vstupní toobe spotřebované a transformovat úlohou hello.</span><span class="sxs-lookup"><span data-stu-id="617fa-112">**Data Streams**: Stream Analytics jobs must include at least one data stream input toobe consumed and transformed by hello job.</span></span> <span data-ttu-id="617fa-113">Azure Blob storage a Azure Event Hubs se podporují jako vstupní zdroje dat datového proudu.</span><span class="sxs-lookup"><span data-stu-id="617fa-113">Azure Blob storage and Azure Event Hubs are supported as data stream input sources.</span></span> <span data-ttu-id="617fa-114">Azure Event Hubs je použité toocollect streamů událostí z připojených zařízení, služeb a aplikací.</span><span class="sxs-lookup"><span data-stu-id="617fa-114">Azure Event Hubs are used toocollect event streams from connected devices, services and applications.</span></span> <span data-ttu-id="617fa-115">Azure Blob storage můžete použít jako vstupní zdroj pro příjem dat hromadné jako datový proud.</span><span class="sxs-lookup"><span data-stu-id="617fa-115">Azure Blob storage can be used as an input source for ingesting bulk data as a stream.</span></span>  
* <span data-ttu-id="617fa-116">**Referenční data**: Stream Analytics podporuje druhého typu pomocného vstupní volané referenční data.</span><span class="sxs-lookup"><span data-stu-id="617fa-116">**Reference data**: Stream Analytics supports a second type of auxiliary input called reference data.</span></span>  <span data-ttu-id="617fa-117">Jako názvem na rozdíl od toodata přesouvaných tato data jsou statická nebo zpomalení změna.</span><span class="sxs-lookup"><span data-stu-id="617fa-117">As opposed toodata in motion, this data is static or slowing changing.</span></span>  <span data-ttu-id="617fa-118">Se obvykle používá k provedení look-ups a korelací s toocreate datové proudy data širší datové sady.</span><span class="sxs-lookup"><span data-stu-id="617fa-118">It is typically used for performing look-ups and correlations with data streams toocreate a richer data set.</span></span>  <span data-ttu-id="617fa-119">Azure Blob storage je aktuálně podporuje jenom hello vstupní zdroj pro referenční data.</span><span class="sxs-lookup"><span data-stu-id="617fa-119">Azure Blob storage is currently hello only supported input source for reference data.</span></span>  

<span data-ttu-id="617fa-120">úlohu Stream Analytics vstupní tooyour tooadd:</span><span class="sxs-lookup"><span data-stu-id="617fa-120">tooadd an input tooyour Stream Analytics job:</span></span>

1. <span data-ttu-id="617fa-121">V hello portálu Azure klikněte na **vstupy** a pak klikněte na **přidat vstup** v úloze Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="617fa-121">In hello Azure portal click **Inputs** and then click **Add an Input** in your Stream Analytics job.</span></span>
   
    ![Portál Azure classic – přidat vstup.](./media/stream-analytics-add-inputs/1-stream-analytics-add-inputs.png)  
   
    <span data-ttu-id="617fa-123">V hello portálu Azure klikněte na tlačítko hello **vstupy** dlaždici v úloze Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="617fa-123">In hello Azure portal click hello **Inputs** tile in your Stream Analytics job.</span></span>  
   
    ![Portál Azure – přidání datového vstupu.](./media/stream-analytics-add-inputs/7-stream-analytics-add-inputs.png)  
2. <span data-ttu-id="617fa-125">Zadejte typ hello hello vstupu: buď **datový proud** nebo **referenční data**.</span><span class="sxs-lookup"><span data-stu-id="617fa-125">Specify hello type of hello input: either **Data stream** or **Reference data**.</span></span>
   
    ![Přidání správná data hello vstup, streamování nebo odkaz](./media/stream-analytics-add-inputs/2-stream-analytics-add-inputs.png)  
   
    ![Přidání správná data hello vstup, streamování nebo odkaz](./media/stream-analytics-add-inputs/8-stream-analytics-add-inputs.png)  
3. <span data-ttu-id="617fa-128">Pokud vytváříte vstupní datový proud, zadejte hello typ zdroje pro vstup hello.</span><span class="sxs-lookup"><span data-stu-id="617fa-128">If creating a Data Stream input, specify hello source type for hello input.</span></span>  <span data-ttu-id="617fa-129">Během vytváření referenční Data jako jenom objekt Blob úložiště je podporována v tuto chvíli můžete tento krok přeskočen.</span><span class="sxs-lookup"><span data-stu-id="617fa-129">This step can be skipped during Reference Data creation as only Blob storage is supported at this time.</span></span>
   
    ![Přidat vstup datového streamu dat](./media/stream-analytics-add-inputs/3-stream-analytics-add-inputs.png)  
   
    ![Přidat vstup datového streamu dat](./media/stream-analytics-add-inputs/9-stream-analytics-add-inputs.png)  
4. <span data-ttu-id="617fa-132">Zadejte popisný název pro tento vstupní v hello vstupní Alias pole.</span><span class="sxs-lookup"><span data-stu-id="617fa-132">Provide a friendly name for this input in hello Input Alias box.</span></span>  <span data-ttu-id="617fa-133">Tento název se použije v dotazu vaše úlohy později na toorefer toohello vstup.</span><span class="sxs-lookup"><span data-stu-id="617fa-133">This name will be used in your job's query later on toorefer toohello input.</span></span>
   
    <span data-ttu-id="617fa-134">Vyplňte hello rest hello požadované připojení vlastnosti zdroje dat tooconnect tooyour.</span><span class="sxs-lookup"><span data-stu-id="617fa-134">Fill in hello rest of hello required connection properties tooconnect tooyour data source.</span></span> <span data-ttu-id="617fa-135">Tato pole se liší podle typu vstupu a zdroj typu a jsou definovány podrobně [zde](stream-analytics-create-a-job.md).</span><span class="sxs-lookup"><span data-stu-id="617fa-135">These fields vary by type of input and source type and are defined in detail [here](stream-analytics-create-a-job.md).</span></span>  
   
    ![Přidat vstup datového centra událostí](./media/stream-analytics-add-inputs/4-stream-analytics-add-inputs.png)  
5. <span data-ttu-id="617fa-137">Zadejte nastavení hello serializace pro vstupní data hello:</span><span class="sxs-lookup"><span data-stu-id="617fa-137">Specify hello serialization settings for hello input data:</span></span>
   
   * <span data-ttu-id="617fa-138">aby dotazy fungovaly hello očekávaným způsobem, toomake zadejte hello **formát serializace událostí** příchozích dat.</span><span class="sxs-lookup"><span data-stu-id="617fa-138">toomake sure your queries work hello way you expect, specify hello **Event Serialization Format** of incoming data.</span></span>  <span data-ttu-id="617fa-139">Formáty podporované serializace jsou JSON, CSV a Avro.</span><span class="sxs-lookup"><span data-stu-id="617fa-139">Supported serialization formats are JSON, CSV, and Avro.</span></span>
   * <span data-ttu-id="617fa-140">Ověřte hello **kódování** pro hello data.</span><span class="sxs-lookup"><span data-stu-id="617fa-140">Verify hello **Encoding** for hello data.</span></span>  <span data-ttu-id="617fa-141">Znakové sady UTF-8 je hello pouze v současné době podporovaný formát kódování.</span><span class="sxs-lookup"><span data-stu-id="617fa-141">UTF-8 is hello only supported encoding format at this time.</span></span>
     
     ![Nastavení serializace dat pro vstupní data hello](./media/stream-analytics-add-inputs/5-stream-analytics-add-inputs.png)  
     
     ![Nastavení serializace dat pro vstupní data hello](./media/stream-analytics-add-inputs/10-stream-analytics-add-inputs.png)  
6. <span data-ttu-id="617fa-144">Po dokončení vytvoření vstupní hello, Stream Analytics ověří, zda se může připojit toohello vstupní zdroj.</span><span class="sxs-lookup"><span data-stu-id="617fa-144">After completing hello input creation, Stream Analytics will verify that it can connect toohello input source.</span></span>  <span data-ttu-id="617fa-145">Stav hello hello operace testování připojení můžete zobrazit v centru oznámení hello.</span><span class="sxs-lookup"><span data-stu-id="617fa-145">You can view hello status of hello Test Connection operation in hello Notification hub.</span></span>
   
    ![Testovací připojení hello dat vstupních datových proudů](./media/stream-analytics-add-inputs/6-stream-analytics-add-inputs.png)  
   
    ![Testovací připojení hello dat vstupních datových proudů](./media/stream-analytics-add-inputs/11-stream-analytics-add-inputs.png)  

## <a name="get-help-with-streaming-data-inputs"></a><span data-ttu-id="617fa-148">Získat pomoc s streamování vstupů dat.</span><span class="sxs-lookup"><span data-stu-id="617fa-148">Get help with streaming data inputs</span></span>
<span data-ttu-id="617fa-149">Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="617fa-149">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="617fa-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="617fa-150">Next steps</span></span>
* [<span data-ttu-id="617fa-151">Úvod tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="617fa-151">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="617fa-152">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="617fa-152">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="617fa-153">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="617fa-153">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="617fa-154">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="617fa-154">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="617fa-155">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="617fa-155">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

