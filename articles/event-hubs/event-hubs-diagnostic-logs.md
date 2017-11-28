---
title: "diagnostické protokoly aaaAzure Event Hubs | Microsoft Docs"
description: "Zjistěte, jak tooset až diagnostické protokoly pro event hubs v Azure."
keywords: 
documentationcenter: 
services: event-hubs
author: banisadr
manager: 
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/27/2017
ms.author: sethm;babanisa
ms.openlocfilehash: d2054e2e444e715e5077fe2608fe1e009e6c1d84
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-diagnostic-logs"></a><span data-ttu-id="33afb-103">Diagnostické protokoly událostí rozbočovače</span><span class="sxs-lookup"><span data-stu-id="33afb-103">Event Hubs diagnostic logs</span></span>

<span data-ttu-id="33afb-104">Pro Azure Event Hubs můžete zobrazit dva typy protokolů:</span><span class="sxs-lookup"><span data-stu-id="33afb-104">You can view two types of logs for Azure Event Hubs:</span></span>
* <span data-ttu-id="33afb-105">**[Protokoly aktivity](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)**.</span><span class="sxs-lookup"><span data-stu-id="33afb-105">**[Activity logs](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)**.</span></span> <span data-ttu-id="33afb-106">Tyto protokoly mít informace o operacích na úlohu provést.</span><span class="sxs-lookup"><span data-stu-id="33afb-106">These logs have information about operations performed on a job.</span></span> <span data-ttu-id="33afb-107">Hello protokoly jsou vždy povolena.</span><span class="sxs-lookup"><span data-stu-id="33afb-107">hello logs are always enabled.</span></span>
* <span data-ttu-id="33afb-108">**[Diagnostické protokoly](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)**.</span><span class="sxs-lookup"><span data-stu-id="33afb-108">**[Diagnostic logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)**.</span></span> <span data-ttu-id="33afb-109">Můžete nakonfigurovat diagnostické protokoly pro širší zobrazení všechno, co se děje s úlohou.</span><span class="sxs-lookup"><span data-stu-id="33afb-109">You can configure diagnostic logs for a richer view of everything that happens with a job.</span></span> <span data-ttu-id="33afb-110">Diagnostické protokoly titulní aktivity z hello, když se vytvoří úloha hello až do odstranění hello úlohy, včetně aktualizací a aktivity, ke kterým dojde během hello úloha běží.</span><span class="sxs-lookup"><span data-stu-id="33afb-110">Diagnostic logs cover activities from hello time hello job is created until hello job is deleted, including updates and activities that occur while hello job is running.</span></span>

## <a name="turn-on-diagnostic-logs"></a><span data-ttu-id="33afb-111">Zapnout diagnostické protokoly</span><span class="sxs-lookup"><span data-stu-id="33afb-111">Turn on diagnostic logs</span></span>
<span data-ttu-id="33afb-112">Diagnostické protokoly jsou zakázané ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="33afb-112">Diagnostics logs are disabled by default.</span></span> <span data-ttu-id="33afb-113">diagnostické protokoly tooenable:</span><span class="sxs-lookup"><span data-stu-id="33afb-113">tooenable diagnostic logs:</span></span>

1.  <span data-ttu-id="33afb-114">V hello [portál Azure](https://portal.azure.com)v části **monitorování + správu**, klikněte na tlačítko **protokolů diagnostiky**.</span><span class="sxs-lookup"><span data-stu-id="33afb-114">In hello [Azure portal](https://portal.azure.com), under **Monitoring + Management**, click **Diagnostics logs**.</span></span>

    ![Protokoly toodiagnostic navigačním okně](./media/event-hubs-diagnostic-logs/image1.png)

2.  <span data-ttu-id="33afb-116">Klikněte na tlačítko hello prostředků, které chcete toomonitor.</span><span class="sxs-lookup"><span data-stu-id="33afb-116">Click hello resource you want toomonitor.</span></span>

3.  <span data-ttu-id="33afb-117">Klikněte na tlačítko **zapněte diagnostiku**.</span><span class="sxs-lookup"><span data-stu-id="33afb-117">Click **Turn on diagnostics**.</span></span>

    ![Zapnout diagnostické protokoly](./media/event-hubs-diagnostic-logs/image2.png)

4.  <span data-ttu-id="33afb-119">Pro **stav**, klikněte na tlačítko **na**.</span><span class="sxs-lookup"><span data-stu-id="33afb-119">For **Status**, click **On**.</span></span>

    ![Změnit stav hello diagnostických protokolů](./media/event-hubs-diagnostic-logs/image3.png)

5.  <span data-ttu-id="33afb-121">Sada hello archivu cíl, který chcete zjistit. například účet úložiště, centra událostí nebo analýza protokolů Azure.</span><span class="sxs-lookup"><span data-stu-id="33afb-121">Set hello archive target that you want; for example, a storage account, an event hub, or Azure Log Analytics.</span></span>

6.  <span data-ttu-id="33afb-122">Uložte nové nastavení diagnostiky hello.</span><span class="sxs-lookup"><span data-stu-id="33afb-122">Save hello new diagnostics settings.</span></span>

<span data-ttu-id="33afb-123">Nové nastavení se projeví ve přibližně 10 minut.</span><span class="sxs-lookup"><span data-stu-id="33afb-123">New settings take effect in about 10 minutes.</span></span> <span data-ttu-id="33afb-124">Potom protokolů se objeví v archivace cílové hello nakonfigurované na hello **protokolů diagnostiky** okno.</span><span class="sxs-lookup"><span data-stu-id="33afb-124">After that, logs appear in hello configured archival target, on hello **Diagnostics logs** blade.</span></span>

<span data-ttu-id="33afb-125">Další informace o konfiguraci diagnostiky najdete v tématu hello [přehled Azure diagnostické protokoly](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).</span><span class="sxs-lookup"><span data-stu-id="33afb-125">For more information about configuring diagnostics, see hello [overview of Azure diagnostic logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).</span></span>

## <a name="diagnostic-logs-categories"></a><span data-ttu-id="33afb-126">Diagnostické protokoly kategorií</span><span class="sxs-lookup"><span data-stu-id="33afb-126">Diagnostic logs categories</span></span>
<span data-ttu-id="33afb-127">Centra událostí jsou zaznamenány diagnostické protokoly pro dvou kategorií:</span><span class="sxs-lookup"><span data-stu-id="33afb-127">Event Hubs captures diagnostic logs for two categories:</span></span>

* <span data-ttu-id="33afb-128">**ArchiveLogs**: protokoly související s archivy tooEvent rozbočovače, konkrétně protokoly tooarchive související chyby.</span><span class="sxs-lookup"><span data-stu-id="33afb-128">**ArchiveLogs**: logs related tooEvent Hubs archives, specifically, logs related tooarchive errors.</span></span>
* <span data-ttu-id="33afb-129">**OperationalLogs**: informace o co se děje během operace služby Event Hubs, konkrétně hello typ operace, včetně vytváření centra událostí, prostředky používá a hello stav operace hello.</span><span class="sxs-lookup"><span data-stu-id="33afb-129">**OperationalLogs**: information about what is happening during Event Hubs operations, specifically, hello operation type, including event hub creation, resources used, and hello status of hello operation.</span></span>

## <a name="diagnostic-logs-schema"></a><span data-ttu-id="33afb-130">Diagnostické protokoly schématu</span><span class="sxs-lookup"><span data-stu-id="33afb-130">Diagnostic logs schema</span></span>
<span data-ttu-id="33afb-131">Všechny protokoly se ukládají ve formátu JavaScript Object Notation (JSON).</span><span class="sxs-lookup"><span data-stu-id="33afb-131">All logs are stored in JavaScript Object Notation (JSON) format.</span></span> <span data-ttu-id="33afb-132">Každá položka má polí s řetězcem, které používají formát hello je popsaný v následující části hello.</span><span class="sxs-lookup"><span data-stu-id="33afb-132">Each entry has string fields that use hello format described in hello following sections.</span></span>

### <a name="archive-logs-schema"></a><span data-ttu-id="33afb-133">Schéma protokoly archivu</span><span class="sxs-lookup"><span data-stu-id="33afb-133">Archive logs schema</span></span>

<span data-ttu-id="33afb-134">Řetězce formátu JSON protokolu archivu obsahovat prvky uvedené v následující tabulce hello:</span><span class="sxs-lookup"><span data-stu-id="33afb-134">Archive log JSON strings include elements listed in hello following table:</span></span>

<span data-ttu-id="33afb-135">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="33afb-135">Name</span></span> | <span data-ttu-id="33afb-136">Popis</span><span class="sxs-lookup"><span data-stu-id="33afb-136">Description</span></span>
------- | -------
<span data-ttu-id="33afb-137">Název úlohy</span><span class="sxs-lookup"><span data-stu-id="33afb-137">TaskName</span></span> | <span data-ttu-id="33afb-138">Popis hello úlohu, která se nezdařila.</span><span class="sxs-lookup"><span data-stu-id="33afb-138">Description of hello task that failed.</span></span>
<span data-ttu-id="33afb-139">ID aktivity</span><span class="sxs-lookup"><span data-stu-id="33afb-139">ActivityId</span></span> | <span data-ttu-id="33afb-140">Interní ID použité pro sledování.</span><span class="sxs-lookup"><span data-stu-id="33afb-140">Internal ID, used for tracking.</span></span>
<span data-ttu-id="33afb-141">trackingId</span><span class="sxs-lookup"><span data-stu-id="33afb-141">trackingId</span></span> | <span data-ttu-id="33afb-142">Interní ID použité pro sledování.</span><span class="sxs-lookup"><span data-stu-id="33afb-142">Internal ID, used for tracking.</span></span>
<span data-ttu-id="33afb-143">resourceId</span><span class="sxs-lookup"><span data-stu-id="33afb-143">resourceId</span></span> | <span data-ttu-id="33afb-144">Azure Resource Manager ID prostředku.</span><span class="sxs-lookup"><span data-stu-id="33afb-144">Azure Resource Manager resource ID.</span></span>
<span data-ttu-id="33afb-145">Centrum EventHub</span><span class="sxs-lookup"><span data-stu-id="33afb-145">eventHub</span></span> | <span data-ttu-id="33afb-146">Centra událostí celý název (včetně oboru názvů).</span><span class="sxs-lookup"><span data-stu-id="33afb-146">Event hub full name (includes namespace name).</span></span>
<span data-ttu-id="33afb-147">ID oddílu</span><span class="sxs-lookup"><span data-stu-id="33afb-147">partitionId</span></span> | <span data-ttu-id="33afb-148">Zapisuje do oddílu centra událostí.</span><span class="sxs-lookup"><span data-stu-id="33afb-148">Event Hub partition being written to.</span></span>
<span data-ttu-id="33afb-149">archiveStep</span><span class="sxs-lookup"><span data-stu-id="33afb-149">archiveStep</span></span> | <span data-ttu-id="33afb-150">ArchiveFlushWriter</span><span class="sxs-lookup"><span data-stu-id="33afb-150">ArchiveFlushWriter</span></span>
<span data-ttu-id="33afb-151">startTime</span><span class="sxs-lookup"><span data-stu-id="33afb-151">startTime</span></span> | <span data-ttu-id="33afb-152">Selhání spuštění.</span><span class="sxs-lookup"><span data-stu-id="33afb-152">Failure start time.</span></span>
<span data-ttu-id="33afb-153">selhání</span><span class="sxs-lookup"><span data-stu-id="33afb-153">failures</span></span> | <span data-ttu-id="33afb-154">Počet časy, kdy došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="33afb-154">Number of times failure occurred.</span></span>
<span data-ttu-id="33afb-155">durationInSeconds</span><span class="sxs-lookup"><span data-stu-id="33afb-155">durationInSeconds</span></span> | <span data-ttu-id="33afb-156">Doba trvání selhání.</span><span class="sxs-lookup"><span data-stu-id="33afb-156">Duration of failure.</span></span>
<span data-ttu-id="33afb-157">Zpráva</span><span class="sxs-lookup"><span data-stu-id="33afb-157">message</span></span> | <span data-ttu-id="33afb-158">Chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="33afb-158">Error message.</span></span>
<span data-ttu-id="33afb-159">category</span><span class="sxs-lookup"><span data-stu-id="33afb-159">category</span></span> | <span data-ttu-id="33afb-160">ArchiveLogs</span><span class="sxs-lookup"><span data-stu-id="33afb-160">ArchiveLogs</span></span>

<span data-ttu-id="33afb-161">Hello následující kód je příkladem protokol archivu řetězce formátu JSON:</span><span class="sxs-lookup"><span data-stu-id="33afb-161">hello following code is an example of an archive log JSON string:</span></span>

```json
{
     "TaskName": "EventHubArchiveUserError",
     "ActivityId": "21b89a0b-8095-471a-9db8-d151d74ecf26",
     "trackingId": "21b89a0b-8095-471a-9db8-d151d74ecf26_B7",
     "resourceId": "/SUBSCRIPTIONS/854D368F-1828-428F-8F3C-F2AFFA9B2F7D/RESOURCEGROUPS/DEFAULT-EVENTHUB-CENTRALUS/PROVIDERS/MICROSOFT.EVENTHUB/NAMESPACES/FBETTATI-OPERA-EVENTHUB",
     "eventHub": "fbettati-opera-eventhub:eventhub:eh123~32766",
     "partitionId": "1",
     "archiveStep": "ArchiveFlushWriter",
     "startTime": "9/22/2016 5:11:21 AM",
     "failures": 3,
     "durationInSeconds": 360,
     "message": "Microsoft.WindowsAzure.Storage.StorageException: hello remote server returned an error: (404) Not Found. ---> System.Net.WebException: hello remote server returned an error: (404) Not Found.\r\n   at Microsoft.WindowsAzure.Storage.Shared.Protocol.HttpResponseParsers.ProcessExpectedStatusCodeNoException[T](HttpStatusCode expectedStatusCode, HttpStatusCode actualStatusCode, T retVal, StorageCommandBase`1 cmd, Exception ex)\r\n   at Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob.<PutBlockImpl>b__3e(RESTCommand`1 cmd, HttpWebResponse resp, Exception ex, OperationContext ctx)\r\n   at Microsoft.WindowsAzure.Storage.Core.Executor.Executor.EndGetResponse[T](IAsyncResult getResponseResult)\r\n   --- End of inner exception stack trace ---\r\n   at Microsoft.WindowsAzure.Storage.Core.Util.StorageAsyncResult`1.End()\r\n   at Microsoft.WindowsAzure.Storage.Core.Util.AsyncExtensions.<>c__DisplayClass4.<CreateCallbackVoid>b__3(IAsyncResult ar)\r\n--- End of stack trace from previous location where exception was thrown ---\r\n   at System.",
     "category": "ArchiveLogs"
}
```

### <a name="operational-logs-schema"></a><span data-ttu-id="33afb-162">Schéma operační protokoly</span><span class="sxs-lookup"><span data-stu-id="33afb-162">Operational logs schema</span></span>

<span data-ttu-id="33afb-163">Řetězce formátu JSON operační protokol obsahovat prvky uvedené v následující tabulce hello:</span><span class="sxs-lookup"><span data-stu-id="33afb-163">Operational log JSON strings include elements listed in hello following table:</span></span>

<span data-ttu-id="33afb-164">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="33afb-164">Name</span></span> | <span data-ttu-id="33afb-165">Popis</span><span class="sxs-lookup"><span data-stu-id="33afb-165">Description</span></span>
------- | -------
<span data-ttu-id="33afb-166">ID aktivity</span><span class="sxs-lookup"><span data-stu-id="33afb-166">ActivityId</span></span> | <span data-ttu-id="33afb-167">Vnitřní ID použít tootrack účel.</span><span class="sxs-lookup"><span data-stu-id="33afb-167">Internal ID, used tootrack purpose.</span></span>
<span data-ttu-id="33afb-168">EventName</span><span class="sxs-lookup"><span data-stu-id="33afb-168">EventName</span></span> | <span data-ttu-id="33afb-169">Název operace.</span><span class="sxs-lookup"><span data-stu-id="33afb-169">Operation name.</span></span>  
<span data-ttu-id="33afb-170">resourceId</span><span class="sxs-lookup"><span data-stu-id="33afb-170">resourceId</span></span> | <span data-ttu-id="33afb-171">Azure Resource Manager ID prostředku.</span><span class="sxs-lookup"><span data-stu-id="33afb-171">Azure Resource Manager resource ID.</span></span>
<span data-ttu-id="33afb-172">SubscriptionId</span><span class="sxs-lookup"><span data-stu-id="33afb-172">SubscriptionId</span></span> | <span data-ttu-id="33afb-173">ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="33afb-173">Subscription ID.</span></span>
<span data-ttu-id="33afb-174">EventTimeString</span><span class="sxs-lookup"><span data-stu-id="33afb-174">EventTimeString</span></span> | <span data-ttu-id="33afb-175">Operace čas.</span><span class="sxs-lookup"><span data-stu-id="33afb-175">Operation time.</span></span>
<span data-ttu-id="33afb-176">EventProperties</span><span class="sxs-lookup"><span data-stu-id="33afb-176">EventProperties</span></span> | <span data-ttu-id="33afb-177">Vlastnosti operace.</span><span class="sxs-lookup"><span data-stu-id="33afb-177">Operation properties.</span></span>
<span data-ttu-id="33afb-178">Status</span><span class="sxs-lookup"><span data-stu-id="33afb-178">Status</span></span> | <span data-ttu-id="33afb-179">Stav operace.</span><span class="sxs-lookup"><span data-stu-id="33afb-179">Operation status.</span></span>
<span data-ttu-id="33afb-180">Volající</span><span class="sxs-lookup"><span data-stu-id="33afb-180">Caller</span></span> | <span data-ttu-id="33afb-181">Volající operace (Azure portal nebo správu klienta).</span><span class="sxs-lookup"><span data-stu-id="33afb-181">Caller of operation (Azure portal or management client).</span></span>
<span data-ttu-id="33afb-182">category</span><span class="sxs-lookup"><span data-stu-id="33afb-182">category</span></span> | <span data-ttu-id="33afb-183">OperationalLogs</span><span class="sxs-lookup"><span data-stu-id="33afb-183">OperationalLogs</span></span>

<span data-ttu-id="33afb-184">Hello následující kód je příkladem řetězce JSON operační protokol:</span><span class="sxs-lookup"><span data-stu-id="33afb-184">hello following code is an example of an operational log JSON string:</span></span>

```json
Example:
{
     "ActivityId": "6aa994ac-b56e-4292-8448-0767a5657cc7",
     "EventName": "Create EventHub",
     "resourceId": "/SUBSCRIPTIONS/1A2109E3-9DA0-455B-B937-E35E36C1163C/RESOURCEGROUPS/DEFAULT-SERVICEBUS-CENTRALUS/PROVIDERS/MICROSOFT.EVENTHUB/NAMESPACES/SHOEBOXEHNS-CY4001",
     "SubscriptionId": "1a2109e3-9da0-455b-b937-e35e36c1163c",
     "EventTimeString": "9/28/2016 8:40:06 PM +00:00",
     "EventProperties": "{\"SubscriptionId\":\"1a2109e3-9da0-455b-b937-e35e36c1163c\",\"Namespace\":\"shoeboxehns-cy4001\",\"Via\":\"https://shoeboxehns-cy4001.servicebus.windows.net/f8096791adb448579ee83d30e006a13e/?api-version=2016-07\",\"TrackingId\":\"5ee74c9e-72b5-4e98-97c4-08a62e56e221_G1\"}",
     "Status": "Succeeded",
     "Caller": "ServiceBus Client",
     "category": "OperationalLogs"
}
```

## <a name="next-steps"></a><span data-ttu-id="33afb-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="33afb-185">Next steps</span></span>
* [<span data-ttu-id="33afb-186">Úvod tooEvent rozbočovače</span><span class="sxs-lookup"><span data-stu-id="33afb-186">Introduction tooEvent Hubs</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="33afb-187">Přehled rozhraní API služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="33afb-187">Event Hubs API overview</span></span>](event-hubs-api-overview.md)
* [<span data-ttu-id="33afb-188">Začínáme s Event Hubs</span><span class="sxs-lookup"><span data-stu-id="33afb-188">Get started with Event Hubs</span></span>](event-hubs-csharp-ephcs-getstarted.md)
