---
title: "aaaEnable diagnostické protokolování pro události Batch - Azure | Microsoft Docs"
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
ms.openlocfilehash: 9d03303a3e857e9303f40cc6de5c32b5a51d8f8a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="log-events-for-diagnostic-evaluation-and-monitoring-of-batch-solutions"></a><span data-ttu-id="cd8e5-103">Události protokolu diagnostiky hodnocení a sledování řešení Batch</span><span class="sxs-lookup"><span data-stu-id="cd8e5-103">Log events for diagnostic evaluation and monitoring of Batch solutions</span></span>

<span data-ttu-id="cd8e5-104">Stejně jako u řadou služeb Azure, hello služba Batch vysílá události protokolu pro některé prostředky během hello doba platnosti prostředku hello.</span><span class="sxs-lookup"><span data-stu-id="cd8e5-104">As with many Azure services, hello Batch service emits log events for certain resources during hello lifetime of hello resource.</span></span> <span data-ttu-id="cd8e5-105">Můžete povolit Azure Batch diagnostické protokoly událostí toorecord prostředkům, jako jsou fondy a úkoly a pak použít hello protokolů diagnostiky hodnocení a sledování.</span><span class="sxs-lookup"><span data-stu-id="cd8e5-105">You can enable Azure Batch diagnostic logs toorecord events for resources like pools and tasks, and then use hello logs for diagnostic evaluation and monitoring.</span></span> <span data-ttu-id="cd8e5-106">Vytvořit událostmi, jako je fondu, fond odstranit, spuštění úloh, úloha ukončena a jiné jsou součástí Batch diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="cd8e5-106">Events like pool create, pool delete, task start, task complete, and others are included in Batch diagnostic logs.</span></span>

> [!NOTE]
> <span data-ttu-id="cd8e5-107">Tento článek popisuje protokolování událostí pro prostředky na účtu Batch, sami, není úloh a úloh výstupní data.</span><span class="sxs-lookup"><span data-stu-id="cd8e5-107">This article discusses logging events for Batch account resources themselves, not job and task output data.</span></span> <span data-ttu-id="cd8e5-108">Informace o ukládání hello výstupních dat úloh a úloh, najdete v tématu [zachovat Azure Batch výstup úlohy a úkolů](batch-task-output.md).</span><span class="sxs-lookup"><span data-stu-id="cd8e5-108">For details on storing hello output data of your jobs and tasks, see [Persist Azure Batch job and task output](batch-task-output.md).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="cd8e5-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cd8e5-109">Prerequisites</span></span>
* [<span data-ttu-id="cd8e5-110">Účet Azure Batch</span><span class="sxs-lookup"><span data-stu-id="cd8e5-110">Azure Batch account</span></span>](batch-account-create-portal.md)
* [<span data-ttu-id="cd8e5-111">účet služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="cd8e5-111">Azure Storage account</span></span>](../storage/common/storage-create-storage-account.md#create-a-storage-account)
  
  <span data-ttu-id="cd8e5-112">diagnostické protokoly toopersist Batch, musíte vytvořit účet úložiště Azure, kde Azure ukládat protokoly hello.</span><span class="sxs-lookup"><span data-stu-id="cd8e5-112">toopersist Batch diagnostic logs, you must create an Azure Storage account where Azure will store hello logs.</span></span> <span data-ttu-id="cd8e5-113">Zadejte účet úložiště při jste [povolit protokolování diagnostiky](#enable-diagnostic-logging) vašeho účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="cd8e5-113">You specify this Storage account when you [enable diagnostic logging](#enable-diagnostic-logging) for your Batch account.</span></span> <span data-ttu-id="cd8e5-114">Hello účet úložiště, které zadáte, když povolíte protokol kolekce není hello stejné jako propojené úložiště účet odkazované tooin hello [balíčky aplikací](batch-application-packages.md) a [úloh výstup trvalost](batch-task-output.md) články.</span><span class="sxs-lookup"><span data-stu-id="cd8e5-114">hello Storage account you specify when you enable log collection is not hello same as a linked storage account referred tooin hello [application packages](batch-application-packages.md) and [task output persistence](batch-task-output.md) articles.</span></span>
  
  > [!WARNING]
  > <span data-ttu-id="cd8e5-115">Jste **účtovat** hello data uložená v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="cd8e5-115">You are **charged** for hello data stored in your Azure Storage account.</span></span> <span data-ttu-id="cd8e5-116">To zahrnuje hello diagnostické protokoly popsané v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="cd8e5-116">This includes hello diagnostic logs discussed in this article.</span></span> <span data-ttu-id="cd8e5-117">Mějte na paměti při navrhování vaší [protokolu zásady uchovávání informací](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md).</span><span class="sxs-lookup"><span data-stu-id="cd8e5-117">Keep this in mind when designing your [log retention policy](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md).</span></span>
  > 
  > 

## <a name="enable-diagnostic-logging"></a><span data-ttu-id="cd8e5-118">Povolení protokolování diagnostiky</span><span class="sxs-lookup"><span data-stu-id="cd8e5-118">Enable diagnostic logging</span></span>
<span data-ttu-id="cd8e5-119">Ve výchozím nastavení vašeho účtu Batch není povoleno protokolování diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="cd8e5-119">Diagnostic logging is not enabled by default for your Batch account.</span></span> <span data-ttu-id="cd8e5-120">Je potřeba explicitně povolit protokolování diagnostiky pro každý účet Batch, že chcete toomonitor:</span><span class="sxs-lookup"><span data-stu-id="cd8e5-120">You must explicitly enable diagnostic logging for each Batch account you want toomonitor:</span></span>

[<span data-ttu-id="cd8e5-121">Jak tooenable shromažďování diagnostických protokolů</span><span class="sxs-lookup"><span data-stu-id="cd8e5-121">How tooenable collection of Diagnostic Logs</span></span>](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs)

<span data-ttu-id="cd8e5-122">Doporučujeme, abyste si přečetli hello úplné [přehled o Azure diagnostické protokoly](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) toogain článku pochopení nejen jak tooenable protokolování, ale hello protokolu kategorií nepodporuje hello různé služby Azure.</span><span class="sxs-lookup"><span data-stu-id="cd8e5-122">We recommend that you read hello full [Overview of Azure Diagnostic Logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) article toogain an understanding of not only how tooenable logging, but hello log categories supported by hello various Azure services.</span></span> <span data-ttu-id="cd8e5-123">Například Azure Batch aktuálně podporuje jednu kategorii protokolu: **protokoly služby**.</span><span class="sxs-lookup"><span data-stu-id="cd8e5-123">For example, Azure Batch currently supports one log category: **Service Logs**.</span></span>

## <a name="service-logs"></a><span data-ttu-id="cd8e5-124">Protokoly služby</span><span class="sxs-lookup"><span data-stu-id="cd8e5-124">Service Logs</span></span>
<span data-ttu-id="cd8e5-125">Protokoly služby Azure Batch obsahovat události vygenerované službou Azure Batch hello během doby života hello prostředku Batch jako fondu nebo úloh.</span><span class="sxs-lookup"><span data-stu-id="cd8e5-125">Azure Batch Service Logs contain events emitted by hello Azure Batch service during hello lifetime of a Batch resource like a pool or task.</span></span> <span data-ttu-id="cd8e5-126">Každá událost vysílaných Batch je uložen v hello zadaný účet úložiště ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="cd8e5-126">Each event emitted by Batch is stored in hello specified Storage account in JSON format.</span></span> <span data-ttu-id="cd8e5-127">To je třeba textu hello ukázkové **fondu vytvoření události**:</span><span class="sxs-lookup"><span data-stu-id="cd8e5-127">For example, this is hello body of a sample **pool create event**:</span></span>

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

<span data-ttu-id="cd8e5-128">Každé události textu se nachází v .json souboru v hello zadaný účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="cd8e5-128">Each event body resides in a .json file in hello specified Azure Storage account.</span></span> <span data-ttu-id="cd8e5-129">Pokud chcete tooaccess hello protokoly přímo, můžete tooreview hello [schéma diagnostických protokolů v účtu úložiště hello](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md#schema-of-diagnostic-logs-in-the-storage-account).</span><span class="sxs-lookup"><span data-stu-id="cd8e5-129">If you want tooaccess hello logs directly, you may wish tooreview hello [schema of Diagnostic Logs in hello storage account](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md#schema-of-diagnostic-logs-in-the-storage-account).</span></span>

## <a name="service-log-events"></a><span data-ttu-id="cd8e5-130">Služba Protokol událostí</span><span class="sxs-lookup"><span data-stu-id="cd8e5-130">Service Log events</span></span>
<span data-ttu-id="cd8e5-131">Hello služba Batch aktuálně vysílá hello následující služby protokolu událostí.</span><span class="sxs-lookup"><span data-stu-id="cd8e5-131">hello Batch service currently emits hello following Service Log events.</span></span> <span data-ttu-id="cd8e5-132">Tento seznam nemusí být kompletní, protože další události mohly být přidány od poslední aktualizace v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="cd8e5-132">This list may not be exhaustive, since additional events may have been added since this article was last updated.</span></span>

| <span data-ttu-id="cd8e5-133">**Služba Protokol událostí**</span><span class="sxs-lookup"><span data-stu-id="cd8e5-133">**Service Log events**</span></span> |
| --- |
| <span data-ttu-id="cd8e5-134">[Vytvoření fondu][pool_create]</span><span class="sxs-lookup"><span data-stu-id="cd8e5-134">[Pool create][pool_create]</span></span> |
| <span data-ttu-id="cd8e5-135">[Spuštění odstranění fondu][pool_delete_start]</span><span class="sxs-lookup"><span data-stu-id="cd8e5-135">[Pool delete start][pool_delete_start]</span></span> |
| <span data-ttu-id="cd8e5-136">[Odstranění fondu dokončení][pool_delete_complete]</span><span class="sxs-lookup"><span data-stu-id="cd8e5-136">[Pool delete complete][pool_delete_complete]</span></span> |
| <span data-ttu-id="cd8e5-137">[Počáteční velikosti fondu][pool_resize_start]</span><span class="sxs-lookup"><span data-stu-id="cd8e5-137">[Pool resize start][pool_resize_start]</span></span> |
| <span data-ttu-id="cd8e5-138">[Dokončení změny velikosti fondu][pool_resize_complete]</span><span class="sxs-lookup"><span data-stu-id="cd8e5-138">[Pool resize complete][pool_resize_complete]</span></span> |
| <span data-ttu-id="cd8e5-139">[Spuštění úlohy][task_start]</span><span class="sxs-lookup"><span data-stu-id="cd8e5-139">[Task start][task_start]</span></span> |
| <span data-ttu-id="cd8e5-140">[Dokončení úkolů][task_complete]</span><span class="sxs-lookup"><span data-stu-id="cd8e5-140">[Task complete][task_complete]</span></span> |
| <span data-ttu-id="cd8e5-141">[Selhání úlohy][task_fail]</span><span class="sxs-lookup"><span data-stu-id="cd8e5-141">[Task fail][task_fail]</span></span> |

## <a name="next-steps"></a><span data-ttu-id="cd8e5-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cd8e5-142">Next steps</span></span>
<span data-ttu-id="cd8e5-143">Přidání toostoring protokolů diagnostiky událostem v účtu Azure Storage, můžete také stream tooan události protokolu služby Batch [centra událostí Azure](../event-hubs/event-hubs-what-is-event-hubs.md)a odešlete je příliš[Azure Log Analytics](../log-analytics/log-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="cd8e5-143">In addition toostoring diagnostic log events in an Azure Storage account, you can also stream Batch Service Log events tooan [Azure Event Hub](../event-hubs/event-hubs-what-is-event-hubs.md), and send them too[Azure Log Analytics](../log-analytics/log-analytics-overview.md).</span></span>

* [<span data-ttu-id="cd8e5-144">Datový proud diagnostických protokolů Azure tooEvent rozbočovače</span><span class="sxs-lookup"><span data-stu-id="cd8e5-144">Stream Azure Diagnostic Logs tooEvent Hubs</span></span>](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md)
  
  <span data-ttu-id="cd8e5-145">Stream diagnostických událostí toohello vysoce škálovatelné data příjem příchozích dat služby Batch, do centra událostí.</span><span class="sxs-lookup"><span data-stu-id="cd8e5-145">Stream Batch diagnostic events toohello highly scalable data ingress service, Event Hubs.</span></span> <span data-ttu-id="cd8e5-146">Centra událostí můžete přijímat miliony událostí za sekundu, které můžete transformovat a ukládat pomocí libovolného zprostředkovatele analýzu v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="cd8e5-146">Event Hubs can ingest millions of events per second, which you can then transform and store using any real-time analytics provider.</span></span>
* [<span data-ttu-id="cd8e5-147">Analýza Azure diagnostických protokolů pomocí analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="cd8e5-147">Analyze Azure diagnostic logs using Log Analytics</span></span>](../log-analytics/log-analytics-azure-storage.md)
  
  <span data-ttu-id="cd8e5-148">Odeslání vašeho tooLog diagnostické protokoly analýzy, kde můžete analyzovat, portálu hello Operations Management Suite (OMS), nebo exportovat pro analýzu v Power BI nebo Excelu.</span><span class="sxs-lookup"><span data-stu-id="cd8e5-148">Send your diagnostic logs tooLog Analytics where you can analyze them in hello Operations Management Suite (OMS) portal, or export them for analysis in Power BI or Excel.</span></span>

[pool_create]: https://msdn.microsoft.com/library/azure/mt743615.aspx
[pool_delete_start]: https://msdn.microsoft.com/library/azure/mt743610.aspx
[pool_delete_complete]: https://msdn.microsoft.com/library/azure/mt743618.aspx
[pool_resize_start]: https://msdn.microsoft.com/library/azure/mt743609.aspx
[pool_resize_complete]: https://msdn.microsoft.com/library/azure/mt743608.aspx
[task_start]: https://msdn.microsoft.com/library/azure/mt743616.aspx
[task_complete]: https://msdn.microsoft.com/library/azure/mt743612.aspx
[task_fail]: https://msdn.microsoft.com/library/azure/mt743607.aspx
