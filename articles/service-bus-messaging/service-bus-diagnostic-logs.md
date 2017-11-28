---
title: "Diagnostické protokoly služby Azure Service Bus | Microsoft Docs"
description: "Zjistěte, jak pro nastavení diagnostické protokoly pro Service Bus v Azure."
keywords: 
documentationcenter: .net
services: service-bus-messaging
author: banisadr
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/27/2017
ms.author: babanisa;sethm
ms.openlocfilehash: 72e18444c83b84c5191a0aab3dc6983517167dd1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="service-bus-diagnostic-logs"></a><span data-ttu-id="52ec3-103">Diagnostické protokoly služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="52ec3-103">Service Bus diagnostic logs</span></span>

<span data-ttu-id="52ec3-104">Pro Azure Service Bus můžete zobrazit dva typy protokolů:</span><span class="sxs-lookup"><span data-stu-id="52ec3-104">You can view two types of logs for Azure Service Bus:</span></span>
* <span data-ttu-id="52ec3-105">**[Protokoly aktivity](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)**.</span><span class="sxs-lookup"><span data-stu-id="52ec3-105">**[Activity logs](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)**.</span></span> <span data-ttu-id="52ec3-106">Tyto protokoly obsahují informace o operace provedené na úlohu.</span><span class="sxs-lookup"><span data-stu-id="52ec3-106">These logs contain information about operations performed on a job.</span></span> <span data-ttu-id="52ec3-107">Protokoly jsou vždy povolena.</span><span class="sxs-lookup"><span data-stu-id="52ec3-107">The logs are always enabled.</span></span>
* <span data-ttu-id="52ec3-108">**[Diagnostické protokoly](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)**.</span><span class="sxs-lookup"><span data-stu-id="52ec3-108">**[Diagnostic logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)**.</span></span> <span data-ttu-id="52ec3-109">Můžete nakonfigurovat diagnostické protokoly bohatší informace o všechno, co se děje v rámci úlohy.</span><span class="sxs-lookup"><span data-stu-id="52ec3-109">You can configure diagnostic logs for richer information about everything that happens within a job.</span></span> <span data-ttu-id="52ec3-110">Diagnostické protokoly titulní aktivit od okamžiku, kdy úloha je vytvořena až do odstranění úlohy, včetně aktualizací a aktivity, ke kterým dochází při běhu úlohy.</span><span class="sxs-lookup"><span data-stu-id="52ec3-110">Diagnostic logs cover activities from the time the job is created until the job is deleted, including updates and activities that occur while the job is running.</span></span>

## <a name="turn-on-diagnostic-logs"></a><span data-ttu-id="52ec3-111">Zapnout diagnostické protokoly</span><span class="sxs-lookup"><span data-stu-id="52ec3-111">Turn on diagnostic logs</span></span>

<span data-ttu-id="52ec3-112">Diagnostické protokoly jsou zakázané ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="52ec3-112">Diagnostics logs are disabled by default.</span></span> <span data-ttu-id="52ec3-113">Pokud chcete povolit diagnostické protokoly, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="52ec3-113">To enable diagnostic logs, perform the following steps:</span></span>

1.  <span data-ttu-id="52ec3-114">V [portál Azure](https://portal.azure.com)v části **monitorování + správu**, klikněte na tlačítko **protokolů diagnostiky**.</span><span class="sxs-lookup"><span data-stu-id="52ec3-114">In the [Azure portal](https://portal.azure.com), under **Monitoring + Management**, click **Diagnostics logs**.</span></span>

    ![Okno navigace k diagnostickým protokolům](./media/service-bus-diagnostic-logs/image1.png)

2. <span data-ttu-id="52ec3-116">Klikněte na prostředek, který chcete monitorovat.</span><span class="sxs-lookup"><span data-stu-id="52ec3-116">Click the resource you want to monitor.</span></span>  

3.  <span data-ttu-id="52ec3-117">Klikněte na tlačítko **zapněte diagnostiku**.</span><span class="sxs-lookup"><span data-stu-id="52ec3-117">Click **Turn on diagnostics**.</span></span>

    ![Zapnout diagnostické protokoly](./media/service-bus-diagnostic-logs/image2.png)

4.  <span data-ttu-id="52ec3-119">Pro **stav**, klikněte na tlačítko **na**.</span><span class="sxs-lookup"><span data-stu-id="52ec3-119">For **Status**, click **On**.</span></span>

    ![změnit stav diagnostických protokolů](./media/service-bus-diagnostic-logs/image3.png)

5.  <span data-ttu-id="52ec3-121">Nastavit cíl archiv, který chcete zjistit. například účet úložiště, centra událostí nebo analýza protokolů Azure.</span><span class="sxs-lookup"><span data-stu-id="52ec3-121">Set the archive target that you want; for example, a storage account, an Event Hub, or Azure Log Analytics.</span></span>

6.  <span data-ttu-id="52ec3-122">Uložte nové nastavení diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="52ec3-122">Save the new diagnostics settings.</span></span>

<span data-ttu-id="52ec3-123">Nové nastavení se projeví ve přibližně 10 minut.</span><span class="sxs-lookup"><span data-stu-id="52ec3-123">New settings take effect in about 10 minutes.</span></span> <span data-ttu-id="52ec3-124">Potom protokolů se objeví v nakonfigurovaných archivace cíl, na **protokolů diagnostiky** okno.</span><span class="sxs-lookup"><span data-stu-id="52ec3-124">After that, logs appear in the configured archival target, on the **Diagnostics logs** blade.</span></span>

<span data-ttu-id="52ec3-125">Další informace o konfiguraci diagnostiky, najdete v článku [přehled Azure diagnostické protokoly](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).</span><span class="sxs-lookup"><span data-stu-id="52ec3-125">For more information about configuring diagnostics, see the [overview of Azure diagnostic logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).</span></span>

## <a name="diagnostic-logs-schema"></a><span data-ttu-id="52ec3-126">Diagnostické protokoly schématu</span><span class="sxs-lookup"><span data-stu-id="52ec3-126">Diagnostic logs schema</span></span>

<span data-ttu-id="52ec3-127">Všechny protokoly se ukládají ve formátu JavaScript Object Notation (JSON).</span><span class="sxs-lookup"><span data-stu-id="52ec3-127">All logs are stored in JavaScript Object Notation (JSON) format.</span></span> <span data-ttu-id="52ec3-128">Každá položka má pole řetězce, které používají formát popsaný v následující části.</span><span class="sxs-lookup"><span data-stu-id="52ec3-128">Each entry has string fields that use the format described in the following section.</span></span>

## <a name="operational-logs-schema"></a><span data-ttu-id="52ec3-129">Schéma operační protokoly</span><span class="sxs-lookup"><span data-stu-id="52ec3-129">Operational logs schema</span></span>

<span data-ttu-id="52ec3-130">Přihlásí **OperationalLogs** kategorie zachycení, co se stane, že během operace služby Service Bus.</span><span class="sxs-lookup"><span data-stu-id="52ec3-130">Logs in the **OperationalLogs** category capture what happens during Service Bus operations.</span></span> <span data-ttu-id="52ec3-131">Konkrétně tyto protokoly zachytit typ operace, včetně vytváření fronty, prostředky využívané a stav operace.</span><span class="sxs-lookup"><span data-stu-id="52ec3-131">Specifically, these logs capture the operation type, including queue creation, resources used, and the status of the operation.</span></span>

<span data-ttu-id="52ec3-132">Řetězce formátu JSON operační protokol obsahovat prvky uvedené v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="52ec3-132">Operational log JSON strings include elements listed in the following table:</span></span>

<span data-ttu-id="52ec3-133">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="52ec3-133">Name</span></span> | <span data-ttu-id="52ec3-134">Popis</span><span class="sxs-lookup"><span data-stu-id="52ec3-134">Description</span></span>
------- | -------
<span data-ttu-id="52ec3-135">ID aktivity</span><span class="sxs-lookup"><span data-stu-id="52ec3-135">ActivityId</span></span> | <span data-ttu-id="52ec3-136">Interní ID, použité pro sledování</span><span class="sxs-lookup"><span data-stu-id="52ec3-136">Internal ID, used for tracking</span></span>
<span data-ttu-id="52ec3-137">EventName</span><span class="sxs-lookup"><span data-stu-id="52ec3-137">EventName</span></span> | <span data-ttu-id="52ec3-138">Název operace</span><span class="sxs-lookup"><span data-stu-id="52ec3-138">Operation name</span></span>           
<span data-ttu-id="52ec3-139">resourceId</span><span class="sxs-lookup"><span data-stu-id="52ec3-139">resourceId</span></span> | <span data-ttu-id="52ec3-140">ID prostředku Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="52ec3-140">Azure Resource Manager resource ID</span></span>
<span data-ttu-id="52ec3-141">SubscriptionId</span><span class="sxs-lookup"><span data-stu-id="52ec3-141">SubscriptionId</span></span> | <span data-ttu-id="52ec3-142">ID předplatného</span><span class="sxs-lookup"><span data-stu-id="52ec3-142">Subscription ID</span></span>
<span data-ttu-id="52ec3-143">EventTimeString</span><span class="sxs-lookup"><span data-stu-id="52ec3-143">EventTimeString</span></span> | <span data-ttu-id="52ec3-144">Operace čas</span><span class="sxs-lookup"><span data-stu-id="52ec3-144">Operation time</span></span>
<span data-ttu-id="52ec3-145">EventProperties</span><span class="sxs-lookup"><span data-stu-id="52ec3-145">EventProperties</span></span> | <span data-ttu-id="52ec3-146">Vlastnosti operace</span><span class="sxs-lookup"><span data-stu-id="52ec3-146">Operation properties</span></span>
<span data-ttu-id="52ec3-147">Status</span><span class="sxs-lookup"><span data-stu-id="52ec3-147">Status</span></span> | <span data-ttu-id="52ec3-148">Stav operace</span><span class="sxs-lookup"><span data-stu-id="52ec3-148">Operation status</span></span>
<span data-ttu-id="52ec3-149">Volající</span><span class="sxs-lookup"><span data-stu-id="52ec3-149">Caller</span></span> | <span data-ttu-id="52ec3-150">Volající operace (Azure portal nebo správu klienta)</span><span class="sxs-lookup"><span data-stu-id="52ec3-150">Caller of operation (Azure portal or management client)</span></span>
<span data-ttu-id="52ec3-151">category</span><span class="sxs-lookup"><span data-stu-id="52ec3-151">category</span></span> | <span data-ttu-id="52ec3-152">OperationalLogs</span><span class="sxs-lookup"><span data-stu-id="52ec3-152">OperationalLogs</span></span>

<span data-ttu-id="52ec3-153">Tady je příklad protokol provozní řetězce formátu JSON:</span><span class="sxs-lookup"><span data-stu-id="52ec3-153">Here's an example of an operational log JSON string:</span></span>

```json
{
  "ActivityId": "6aa994ac-b56e-4292-8448-0767a5657cc7",
  "EventName": "Create Queue",
  "resourceId": "/SUBSCRIPTIONS/1A2109E3-9DA0-455B-B937-E35E36C1163C/RESOURCEGROUPS/DEFAULT-SERVICEBUS-CENTRALUS/PROVIDERS/MICROSOFT.SERVICEBUS/NAMESPACES/SHOEBOXEHNS-CY4001",
  "SubscriptionId": "1a2109e3-9da0-455b-b937-e35e36c1163c",
  "EventTimeString": "9/28/2016 8:40:06 PM +00:00",
  "EventProperties": "{\"SubscriptionId\":\"1a2109e3-9da0-455b-b937-e35e36c1163c\",\"Namespace\":\"shoeboxehns-cy4001\",\"Via\":\"https://shoeboxehns-cy4001.servicebus.windows.net/f8096791adb448579ee83d30e006a13e/?api-version=2016-07\",\"TrackingId\":\"5ee74c9e-72b5-4e98-97c4-08a62e56e221_G1\"}",
  "Status": "Succeeded",
  "Caller": "ServiceBus Client",
  "category": "OperationalLogs"
}
```

## <a name="next-steps"></a><span data-ttu-id="52ec3-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="52ec3-154">Next steps</span></span>

<span data-ttu-id="52ec3-155">Po kliknutí následující odkazy na další informace o službě Service Bus:</span><span class="sxs-lookup"><span data-stu-id="52ec3-155">Visit the following links to learn more about Service Bus:</span></span>

* [<span data-ttu-id="52ec3-156">Úvod do služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="52ec3-156">Introduction to Service Bus</span></span>](service-bus-messaging-overview.md)
* [<span data-ttu-id="52ec3-157">Začínáme se službou Service Bus</span><span class="sxs-lookup"><span data-stu-id="52ec3-157">Get started with Service Bus</span></span>](service-bus-dotnet-get-started-with-queues.md)
