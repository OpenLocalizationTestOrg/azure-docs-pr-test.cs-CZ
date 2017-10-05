---
title: "Povolit protokolování diagnostiky pro Batch události - Azure | Microsoft Docs"
description: "Zaznamenávat a analyzovat události protokolů diagnostiky pro prostředkům účet Azure Batch, jako jsou fondy a úkoly."
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: e14e611d-12cd-4671-91dc-bc506dc853e5
ms.service: batch
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: big-compute
ms.date: 05/22/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b7bc6fd9921ab0f2374ace33ea5c1ab93a78f860
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="log-events-for-diagnostic-evaluation-and-monitoring-of-batch-solutions"></a><span data-ttu-id="ffcc2-103">Události protokolu diagnostiky hodnocení a sledování řešení Batch</span><span class="sxs-lookup"><span data-stu-id="ffcc2-103">Log events for diagnostic evaluation and monitoring of Batch solutions</span></span>

<span data-ttu-id="ffcc2-104">Stejně jako u řadou služeb Azure, služba Batch vysílá události protokolu určitých prostředků během doby platnosti prostředku.</span><span class="sxs-lookup"><span data-stu-id="ffcc2-104">As with many Azure services, the Batch service emits log events for certain resources during the lifetime of the resource.</span></span> <span data-ttu-id="ffcc2-105">Můžete povolení diagnostických protokolů Azure Batch zaznamenat události pro prostředkům, jako jsou fondy a úkoly a pak použít protokoly diagnostiky hodnocení a sledování.</span><span class="sxs-lookup"><span data-stu-id="ffcc2-105">You can enable Azure Batch diagnostic logs to record events for resources like pools and tasks, and then use the logs for diagnostic evaluation and monitoring.</span></span> <span data-ttu-id="ffcc2-106">Vytvořit událostmi, jako je fondu, fond odstranit, spuštění úloh, úloha ukončena a jiné jsou součástí Batch diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="ffcc2-106">Events like pool create, pool delete, task start, task complete, and others are included in Batch diagnostic logs.</span></span>

> [!NOTE]
> <span data-ttu-id="ffcc2-107">Tento článek popisuje protokolování událostí pro prostředky na účtu Batch, sami, není úloh a úloh výstupní data.</span><span class="sxs-lookup"><span data-stu-id="ffcc2-107">This article discusses logging events for Batch account resources themselves, not job and task output data.</span></span> <span data-ttu-id="ffcc2-108">Informace o ukládání výstupních dat úloh a úloh, najdete v tématu [zachovat Azure Batch výstup úlohy a úkolů](batch-task-output.md).</span><span class="sxs-lookup"><span data-stu-id="ffcc2-108">For details on storing the output data of your jobs and tasks, see [Persist Azure Batch job and task output](batch-task-output.md).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="ffcc2-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ffcc2-109">Prerequisites</span></span>
* [<span data-ttu-id="ffcc2-110">Účet Azure Batch</span><span class="sxs-lookup"><span data-stu-id="ffcc2-110">Azure Batch account</span></span>](batch-account-create-portal.md)
* [<span data-ttu-id="ffcc2-111">účet služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="ffcc2-111">Azure Storage account</span></span>](../storage/common/storage-create-storage-account.md#create-a-storage-account)
  
  <span data-ttu-id="ffcc2-112">Udržení diagnostické protokoly Batch, musíte vytvořit účet úložiště Azure, kde Azure ukládat protokoly.</span><span class="sxs-lookup"><span data-stu-id="ffcc2-112">To persist Batch diagnostic logs, you must create an Azure Storage account where Azure will store the logs.</span></span> <span data-ttu-id="ffcc2-113">Zadejte účet úložiště při jste [povolit protokolování diagnostiky](#enable-diagnostic-logging) vašeho účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="ffcc2-113">You specify this Storage account when you [enable diagnostic logging](#enable-diagnostic-logging) for your Batch account.</span></span> <span data-ttu-id="ffcc2-114">Účet úložiště, který zadáte, když povolíte protokol kolekce není stejný jako propojený účet úložiště uvedené v [balíčky aplikací](batch-application-packages.md) a [úloh výstup trvalost](batch-task-output.md) články.</span><span class="sxs-lookup"><span data-stu-id="ffcc2-114">The Storage account you specify when you enable log collection is not the same as a linked storage account referred to in the [application packages](batch-application-packages.md) and [task output persistence](batch-task-output.md) articles.</span></span>
  
  > [!WARNING]
  > <span data-ttu-id="ffcc2-115">Jste **účtovat** data uložená v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="ffcc2-115">You are **charged** for the data stored in your Azure Storage account.</span></span> <span data-ttu-id="ffcc2-116">To zahrnuje diagnostických protokolů, které jsou popsané v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="ffcc2-116">This includes the diagnostic logs discussed in this article.</span></span> <span data-ttu-id="ffcc2-117">Mějte na paměti při navrhování vaší [protokolu zásady uchovávání informací](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md).</span><span class="sxs-lookup"><span data-stu-id="ffcc2-117">Keep this in mind when designing your [log retention policy](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md).</span></span>
  > 
  > 

## <a name="enable-diagnostic-logging"></a><span data-ttu-id="ffcc2-118">Povolení protokolování diagnostiky</span><span class="sxs-lookup"><span data-stu-id="ffcc2-118">Enable diagnostic logging</span></span>
<span data-ttu-id="ffcc2-119">Ve výchozím nastavení vašeho účtu Batch není povoleno protokolování diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="ffcc2-119">Diagnostic logging is not enabled by default for your Batch account.</span></span> <span data-ttu-id="ffcc2-120">Je potřeba explicitně povolit protokolování diagnostiky pro každého účtu Batch, kterou chcete sledovat:</span><span class="sxs-lookup"><span data-stu-id="ffcc2-120">You must explicitly enable diagnostic logging for each Batch account you want to monitor:</span></span>

[<span data-ttu-id="ffcc2-121">Postup povolení shromažďování diagnostických protokolů</span><span class="sxs-lookup"><span data-stu-id="ffcc2-121">How to enable collection of Diagnostic Logs</span></span>](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs)

<span data-ttu-id="ffcc2-122">Doporučujeme, abyste si přečetli celý [přehled o Azure diagnostické protokoly](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) článku získat představu o není pouze to, jak povolit protokolování, ale kategorií protokolu podporuje různé služby Azure.</span><span class="sxs-lookup"><span data-stu-id="ffcc2-122">We recommend that you read the full [Overview of Azure Diagnostic Logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) article to gain an understanding of not only how to enable logging, but the log categories supported by the various Azure services.</span></span> <span data-ttu-id="ffcc2-123">Například Azure Batch aktuálně podporuje jednu kategorii protokolu: **protokoly služby**.</span><span class="sxs-lookup"><span data-stu-id="ffcc2-123">For example, Azure Batch currently supports one log category: **Service Logs**.</span></span>

## <a name="service-logs"></a><span data-ttu-id="ffcc2-124">Protokoly služby</span><span class="sxs-lookup"><span data-stu-id="ffcc2-124">Service Logs</span></span>
<span data-ttu-id="ffcc2-125">Protokoly služby Azure Batch obsahovat události vygenerované službou Azure Batch po dobu životnosti prostředků Batch jako fondu nebo úloh.</span><span class="sxs-lookup"><span data-stu-id="ffcc2-125">Azure Batch Service Logs contain events emitted by the Azure Batch service during the lifetime of a Batch resource like a pool or task.</span></span> <span data-ttu-id="ffcc2-126">Každá událost vysílaných Batch je uložený v zadaném účtu úložiště ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="ffcc2-126">Each event emitted by Batch is stored in the specified Storage account in JSON format.</span></span> <span data-ttu-id="ffcc2-127">To je třeba do těla ukázku **fondu vytvoření události**:</span><span class="sxs-lookup"><span data-stu-id="ffcc2-127">For example, this is the body of a sample **pool create event**:</span></span>

```json
{
    "poolId": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "4",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicatedComputeNodes": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoscale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

<span data-ttu-id="ffcc2-128">Každé události textu se nachází v soubor .json v zadaném účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="ffcc2-128">Each event body resides in a .json file in the specified Azure Storage account.</span></span> <span data-ttu-id="ffcc2-129">Pokud chcete získat přímo přístup k protokoly, můžete zkontrolovat [schéma diagnostických protokolů v účtu úložiště](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md#schema-of-diagnostic-logs-in-the-storage-account).</span><span class="sxs-lookup"><span data-stu-id="ffcc2-129">If you want to access the logs directly, you may wish to review the [schema of Diagnostic Logs in the storage account](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md#schema-of-diagnostic-logs-in-the-storage-account).</span></span>

## <a name="service-log-events"></a><span data-ttu-id="ffcc2-130">Služba Protokol událostí</span><span class="sxs-lookup"><span data-stu-id="ffcc2-130">Service Log events</span></span>
<span data-ttu-id="ffcc2-131">Služba Batch aktuálně vysílá následující události služby protokolu.</span><span class="sxs-lookup"><span data-stu-id="ffcc2-131">The Batch service currently emits the following Service Log events.</span></span> <span data-ttu-id="ffcc2-132">Tento seznam nemusí být kompletní, protože další události mohly být přidány od poslední aktualizace v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="ffcc2-132">This list may not be exhaustive, since additional events may have been added since this article was last updated.</span></span>

| <span data-ttu-id="ffcc2-133">**Služba Protokol událostí**</span><span class="sxs-lookup"><span data-stu-id="ffcc2-133">**Service Log events**</span></span> |
| --- |
| <span data-ttu-id="ffcc2-134">[Vytvoření fondu][pool_create]</span><span class="sxs-lookup"><span data-stu-id="ffcc2-134">[Pool create][pool_create]</span></span> |
| <span data-ttu-id="ffcc2-135">[Spuštění odstranění fondu][pool_delete_start]</span><span class="sxs-lookup"><span data-stu-id="ffcc2-135">[Pool delete start][pool_delete_start]</span></span> |
| <span data-ttu-id="ffcc2-136">[Odstranění fondu dokončení][pool_delete_complete]</span><span class="sxs-lookup"><span data-stu-id="ffcc2-136">[Pool delete complete][pool_delete_complete]</span></span> |
| <span data-ttu-id="ffcc2-137">[Počáteční velikosti fondu][pool_resize_start]</span><span class="sxs-lookup"><span data-stu-id="ffcc2-137">[Pool resize start][pool_resize_start]</span></span> |
| <span data-ttu-id="ffcc2-138">[Dokončení změny velikosti fondu][pool_resize_complete]</span><span class="sxs-lookup"><span data-stu-id="ffcc2-138">[Pool resize complete][pool_resize_complete]</span></span> |
| <span data-ttu-id="ffcc2-139">[Spuštění úlohy][task_start]</span><span class="sxs-lookup"><span data-stu-id="ffcc2-139">[Task start][task_start]</span></span> |
| <span data-ttu-id="ffcc2-140">[Dokončení úkolů][task_complete]</span><span class="sxs-lookup"><span data-stu-id="ffcc2-140">[Task complete][task_complete]</span></span> |
| <span data-ttu-id="ffcc2-141">[Selhání úlohy][task_fail]</span><span class="sxs-lookup"><span data-stu-id="ffcc2-141">[Task fail][task_fail]</span></span> |

## <a name="next-steps"></a><span data-ttu-id="ffcc2-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ffcc2-142">Next steps</span></span>
<span data-ttu-id="ffcc2-143">Kromě ukládání diagnostických protokolů událostí v účtu Azure Storage, můžete také stream události protokolu služby Batch [centra událostí Azure](../event-hubs/event-hubs-what-is-event-hubs.md)a pošlete je [Azure Log Analytics](../log-analytics/log-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ffcc2-143">In addition to storing diagnostic log events in an Azure Storage account, you can also stream Batch Service Log events to an [Azure Event Hub](../event-hubs/event-hubs-what-is-event-hubs.md), and send them to [Azure Log Analytics](../log-analytics/log-analytics-overview.md).</span></span>

* [<span data-ttu-id="ffcc2-144">Stream Azure diagnostických protokolů do centra událostí</span><span class="sxs-lookup"><span data-stu-id="ffcc2-144">Stream Azure Diagnostic Logs to Event Hubs</span></span>](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md)
  
  <span data-ttu-id="ffcc2-145">Stream diagnostických událostí Batch ke službě příjem příchozích dat vysoce škálovatelné datového centra událostí.</span><span class="sxs-lookup"><span data-stu-id="ffcc2-145">Stream Batch diagnostic events to the highly scalable data ingress service, Event Hubs.</span></span> <span data-ttu-id="ffcc2-146">Centra událostí můžete přijímat miliony událostí za sekundu, které můžete transformovat a ukládat pomocí libovolného zprostředkovatele analýzu v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="ffcc2-146">Event Hubs can ingest millions of events per second, which you can then transform and store using any real-time analytics provider.</span></span>
* [<span data-ttu-id="ffcc2-147">Analýza Azure diagnostických protokolů pomocí analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="ffcc2-147">Analyze Azure diagnostic logs using Log Analytics</span></span>](../log-analytics/log-analytics-azure-storage.md)
  
  <span data-ttu-id="ffcc2-148">Odesílání diagnostických protokolů k analýze protokolů, kde můžete analyzovat na portálu Operations Management Suite (OMS), nebo exportovat pro analýzu v Power BI nebo Excelu.</span><span class="sxs-lookup"><span data-stu-id="ffcc2-148">Send your diagnostic logs to Log Analytics where you can analyze them in the Operations Management Suite (OMS) portal, or export them for analysis in Power BI or Excel.</span></span>

[pool_create]: https://msdn.microsoft.com/library/azure/mt743615.aspx
[pool_delete_start]: https://msdn.microsoft.com/library/azure/mt743610.aspx
[pool_delete_complete]: https://msdn.microsoft.com/library/azure/mt743618.aspx
[pool_resize_start]: https://msdn.microsoft.com/library/azure/mt743609.aspx
[pool_resize_complete]: https://msdn.microsoft.com/library/azure/mt743608.aspx
[task_start]: https://msdn.microsoft.com/library/azure/mt743616.aspx
[task_complete]: https://msdn.microsoft.com/library/azure/mt743612.aspx
[task_fail]: https://msdn.microsoft.com/library/azure/mt743607.aspx
