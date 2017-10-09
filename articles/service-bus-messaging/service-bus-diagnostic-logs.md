---
title: "diagnostické protokoly aaaAzure Service Bus | Microsoft Docs"
description: "Zjistěte, jak tooset až diagnostické protokoly pro Service Bus v Azure."
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
ms.openlocfilehash: e48d6eaba6e865ae39f5b07ed6cd53d74c92e2ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-diagnostic-logs"></a><span data-ttu-id="edb8c-103">Diagnostické protokoly služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="edb8c-103">Service Bus diagnostic logs</span></span>

<span data-ttu-id="edb8c-104">Pro Azure Service Bus můžete zobrazit dva typy protokolů:</span><span class="sxs-lookup"><span data-stu-id="edb8c-104">You can view two types of logs for Azure Service Bus:</span></span>
* <span data-ttu-id="edb8c-105">**[Protokoly aktivity](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)**.</span><span class="sxs-lookup"><span data-stu-id="edb8c-105">**[Activity logs](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)**.</span></span> <span data-ttu-id="edb8c-106">Tyto protokoly obsahují informace o operace provedené na úlohu.</span><span class="sxs-lookup"><span data-stu-id="edb8c-106">These logs contain information about operations performed on a job.</span></span> <span data-ttu-id="edb8c-107">Hello protokoly jsou vždy povolena.</span><span class="sxs-lookup"><span data-stu-id="edb8c-107">hello logs are always enabled.</span></span>
* <span data-ttu-id="edb8c-108">**[Diagnostické protokoly](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)**.</span><span class="sxs-lookup"><span data-stu-id="edb8c-108">**[Diagnostic logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)**.</span></span> <span data-ttu-id="edb8c-109">Můžete nakonfigurovat diagnostické protokoly bohatší informace o všechno, co se děje v rámci úlohy.</span><span class="sxs-lookup"><span data-stu-id="edb8c-109">You can configure diagnostic logs for richer information about everything that happens within a job.</span></span> <span data-ttu-id="edb8c-110">Diagnostické protokoly titulní aktivity z hello, když se vytvoří úloha hello až do odstranění hello úlohy, včetně aktualizací a aktivity, ke kterým dojde během hello úloha běží.</span><span class="sxs-lookup"><span data-stu-id="edb8c-110">Diagnostic logs cover activities from hello time hello job is created until hello job is deleted, including updates and activities that occur while hello job is running.</span></span>

## <a name="turn-on-diagnostic-logs"></a><span data-ttu-id="edb8c-111">Zapnout diagnostické protokoly</span><span class="sxs-lookup"><span data-stu-id="edb8c-111">Turn on diagnostic logs</span></span>

<span data-ttu-id="edb8c-112">Diagnostické protokoly jsou zakázané ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="edb8c-112">Diagnostics logs are disabled by default.</span></span> <span data-ttu-id="edb8c-113">diagnostické protokoly tooenable, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="edb8c-113">tooenable diagnostic logs, perform hello following steps:</span></span>

1.  <span data-ttu-id="edb8c-114">V hello [portál Azure](https://portal.azure.com)v části **monitorování + správu**, klikněte na tlačítko **protokolů diagnostiky**.</span><span class="sxs-lookup"><span data-stu-id="edb8c-114">In hello [Azure portal](https://portal.azure.com), under **Monitoring + Management**, click **Diagnostics logs**.</span></span>

    ![Protokoly toodiagnostic navigačním okně](./media/service-bus-diagnostic-logs/image1.png)

2. <span data-ttu-id="edb8c-116">Klikněte na tlačítko hello prostředků, které chcete toomonitor.</span><span class="sxs-lookup"><span data-stu-id="edb8c-116">Click hello resource you want toomonitor.</span></span>  

3.  <span data-ttu-id="edb8c-117">Klikněte na tlačítko **zapněte diagnostiku**.</span><span class="sxs-lookup"><span data-stu-id="edb8c-117">Click **Turn on diagnostics**.</span></span>

    ![Zapnout diagnostické protokoly](./media/service-bus-diagnostic-logs/image2.png)

4.  <span data-ttu-id="edb8c-119">Pro **stav**, klikněte na tlačítko **na**.</span><span class="sxs-lookup"><span data-stu-id="edb8c-119">For **Status**, click **On**.</span></span>

    ![změnit stav diagnostických protokolů](./media/service-bus-diagnostic-logs/image3.png)

5.  <span data-ttu-id="edb8c-121">Sada hello archivu cíl, který chcete zjistit. například účet úložiště, centra událostí nebo analýza protokolů Azure.</span><span class="sxs-lookup"><span data-stu-id="edb8c-121">Set hello archive target that you want; for example, a storage account, an Event Hub, or Azure Log Analytics.</span></span>

6.  <span data-ttu-id="edb8c-122">Uložte nové nastavení diagnostiky hello.</span><span class="sxs-lookup"><span data-stu-id="edb8c-122">Save hello new diagnostics settings.</span></span>

<span data-ttu-id="edb8c-123">Nové nastavení se projeví ve přibližně 10 minut.</span><span class="sxs-lookup"><span data-stu-id="edb8c-123">New settings take effect in about 10 minutes.</span></span> <span data-ttu-id="edb8c-124">Potom protokolů se objeví v archivace cílové hello nakonfigurované na hello **protokolů diagnostiky** okno.</span><span class="sxs-lookup"><span data-stu-id="edb8c-124">After that, logs appear in hello configured archival target, on hello **Diagnostics logs** blade.</span></span>

<span data-ttu-id="edb8c-125">Další informace o konfiguraci diagnostiky najdete v tématu hello [přehled Azure diagnostické protokoly](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).</span><span class="sxs-lookup"><span data-stu-id="edb8c-125">For more information about configuring diagnostics, see hello [overview of Azure diagnostic logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).</span></span>

## <a name="diagnostic-logs-schema"></a><span data-ttu-id="edb8c-126">Diagnostické protokoly schématu</span><span class="sxs-lookup"><span data-stu-id="edb8c-126">Diagnostic logs schema</span></span>

<span data-ttu-id="edb8c-127">Všechny protokoly se ukládají ve formátu JavaScript Object Notation (JSON).</span><span class="sxs-lookup"><span data-stu-id="edb8c-127">All logs are stored in JavaScript Object Notation (JSON) format.</span></span> <span data-ttu-id="edb8c-128">Každá položka má polí s řetězcem, které používají formát hello je popsaný v následující části hello.</span><span class="sxs-lookup"><span data-stu-id="edb8c-128">Each entry has string fields that use hello format described in hello following section.</span></span>

## <a name="operational-logs-schema"></a><span data-ttu-id="edb8c-129">Schéma operační protokoly</span><span class="sxs-lookup"><span data-stu-id="edb8c-129">Operational logs schema</span></span>

<span data-ttu-id="edb8c-130">Přihlásí hello **OperationalLogs** kategorie zachycení, co se stane, že během operace služby Service Bus.</span><span class="sxs-lookup"><span data-stu-id="edb8c-130">Logs in hello **OperationalLogs** category capture what happens during Service Bus operations.</span></span> <span data-ttu-id="edb8c-131">Tyto protokoly konkrétně zaznamenání hello typ operace, včetně vytváření fronty, prostředky používá a hello stav operace hello.</span><span class="sxs-lookup"><span data-stu-id="edb8c-131">Specifically, these logs capture hello operation type, including queue creation, resources used, and hello status of hello operation.</span></span>

<span data-ttu-id="edb8c-132">Řetězce formátu JSON operační protokol obsahovat prvky uvedené v následující tabulce hello:</span><span class="sxs-lookup"><span data-stu-id="edb8c-132">Operational log JSON strings include elements listed in hello following table:</span></span>

<span data-ttu-id="edb8c-133">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="edb8c-133">Name</span></span> | <span data-ttu-id="edb8c-134">Popis</span><span class="sxs-lookup"><span data-stu-id="edb8c-134">Description</span></span>
------- | -------
<span data-ttu-id="edb8c-135">ID aktivity</span><span class="sxs-lookup"><span data-stu-id="edb8c-135">ActivityId</span></span> | <span data-ttu-id="edb8c-136">Interní ID, použité pro sledování</span><span class="sxs-lookup"><span data-stu-id="edb8c-136">Internal ID, used for tracking</span></span>
<span data-ttu-id="edb8c-137">EventName</span><span class="sxs-lookup"><span data-stu-id="edb8c-137">EventName</span></span> | <span data-ttu-id="edb8c-138">Název operace</span><span class="sxs-lookup"><span data-stu-id="edb8c-138">Operation name</span></span>           
<span data-ttu-id="edb8c-139">resourceId</span><span class="sxs-lookup"><span data-stu-id="edb8c-139">resourceId</span></span> | <span data-ttu-id="edb8c-140">ID prostředku Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="edb8c-140">Azure Resource Manager resource ID</span></span>
<span data-ttu-id="edb8c-141">SubscriptionId</span><span class="sxs-lookup"><span data-stu-id="edb8c-141">SubscriptionId</span></span> | <span data-ttu-id="edb8c-142">ID předplatného</span><span class="sxs-lookup"><span data-stu-id="edb8c-142">Subscription ID</span></span>
<span data-ttu-id="edb8c-143">EventTimeString</span><span class="sxs-lookup"><span data-stu-id="edb8c-143">EventTimeString</span></span> | <span data-ttu-id="edb8c-144">Operace čas</span><span class="sxs-lookup"><span data-stu-id="edb8c-144">Operation time</span></span>
<span data-ttu-id="edb8c-145">EventProperties</span><span class="sxs-lookup"><span data-stu-id="edb8c-145">EventProperties</span></span> | <span data-ttu-id="edb8c-146">Vlastnosti operace</span><span class="sxs-lookup"><span data-stu-id="edb8c-146">Operation properties</span></span>
<span data-ttu-id="edb8c-147">Status</span><span class="sxs-lookup"><span data-stu-id="edb8c-147">Status</span></span> | <span data-ttu-id="edb8c-148">Stav operace</span><span class="sxs-lookup"><span data-stu-id="edb8c-148">Operation status</span></span>
<span data-ttu-id="edb8c-149">Volající</span><span class="sxs-lookup"><span data-stu-id="edb8c-149">Caller</span></span> | <span data-ttu-id="edb8c-150">Volající operace (Azure portal nebo správu klienta)</span><span class="sxs-lookup"><span data-stu-id="edb8c-150">Caller of operation (Azure portal or management client)</span></span>
<span data-ttu-id="edb8c-151">category</span><span class="sxs-lookup"><span data-stu-id="edb8c-151">category</span></span> | <span data-ttu-id="edb8c-152">OperationalLogs</span><span class="sxs-lookup"><span data-stu-id="edb8c-152">OperationalLogs</span></span>

<span data-ttu-id="edb8c-153">Tady je příklad protokol provozní řetězce formátu JSON:</span><span class="sxs-lookup"><span data-stu-id="edb8c-153">Here's an example of an operational log JSON string:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="edb8c-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="edb8c-154">Next steps</span></span>

<span data-ttu-id="edb8c-155">Navštivte hello následující odkazy toolearn více o službě Service Bus:</span><span class="sxs-lookup"><span data-stu-id="edb8c-155">Visit hello following links toolearn more about Service Bus:</span></span>

* [<span data-ttu-id="edb8c-156">Úvod tooService sběrnice</span><span class="sxs-lookup"><span data-stu-id="edb8c-156">Introduction tooService Bus</span></span>](service-bus-messaging-overview.md)
* [<span data-ttu-id="edb8c-157">Začínáme se službou Service Bus</span><span class="sxs-lookup"><span data-stu-id="edb8c-157">Get started with Service Bus</span></span>](service-bus-dotnet-get-started-with-queues.md)
