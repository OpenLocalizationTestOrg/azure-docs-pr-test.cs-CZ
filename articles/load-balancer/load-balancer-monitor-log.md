---
title: "Sledování operací, události a čítače pro vyrovnávání zatížení | Microsoft Docs"
description: "Zjistěte, jak povolit události výstrah a sběru dat stavu stav protokolování pro nástroj pro vyrovnávání zatížení Azure"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: 56656d74-0241-4096-88c8-aa88515d676d
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: 638ecd5e02889bd8cb6e7429dfcec335feaac4a3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="log-analytics-for-azure-load-balancer"></a><span data-ttu-id="646ed-103">Log Analytics pro Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="646ed-103">Log analytics for Azure Load Balancer</span></span>

<span data-ttu-id="646ed-104">Různé typy protokolů v Azure můžete použít ke správě a odstraňování potíží nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="646ed-104">You can use different types of logs in Azure to manage and troubleshoot load balancers.</span></span> <span data-ttu-id="646ed-105">Některé z těchto protokolů jsou přístupné prostřednictvím portálu.</span><span class="sxs-lookup"><span data-stu-id="646ed-105">Some of these logs can be accessed through the portal.</span></span> <span data-ttu-id="646ed-106">Všechny protokoly můžete extrahovat z Azure blob storage a zobrazit v různých nástrojů, jako je například aplikace Excel a PowerBI.</span><span class="sxs-lookup"><span data-stu-id="646ed-106">All logs can be extracted from Azure blob storage, and viewed in different tools, such as Excel and PowerBI.</span></span> <span data-ttu-id="646ed-107">Další informace o různých typech protokolů z níže uvedeného seznamu.</span><span class="sxs-lookup"><span data-stu-id="646ed-107">You can learn more about the different types of logs from the list below.</span></span>

* <span data-ttu-id="646ed-108">**Protokoly auditu:** můžete použít [protokoly auditu Azure](../monitoring-and-diagnostics/insights-debugging-with-events.md) (dříve označované jako operační protokoly) Chcete-li zobrazit všechny operace odeslání vašich předplatných Azure a jejich stav.</span><span class="sxs-lookup"><span data-stu-id="646ed-108">**Audit logs:** You can use [Azure Audit Logs](../monitoring-and-diagnostics/insights-debugging-with-events.md) (formerly known as Operational Logs) to view all operations being submitted to your Azure subscription(s), and their status.</span></span> <span data-ttu-id="646ed-109">Protokoly auditu jsou ve výchozím nastavení povolené a lze zobrazit na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="646ed-109">Audit logs are enabled by default, and can be viewed in the Azure portal.</span></span>
* <span data-ttu-id="646ed-110">**Výstrahy protokoly událostí:** tento protokol můžete zobrazit výstrahy rasied nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="646ed-110">**Alert event logs:** You can use this log to view alerts rasied by the load balancer.</span></span> <span data-ttu-id="646ed-111">Stav nástroje pro vyrovnávání zatížení je shromažďovaných každých pět minut.</span><span class="sxs-lookup"><span data-stu-id="646ed-111">The status for the load balancer is collected every five minutes.</span></span> <span data-ttu-id="646ed-112">Tento protokol je zapsán pouze pokud je vyvolána o událost výstrahy nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="646ed-112">This log is only written if a load balancer alert event is raised.</span></span>
* <span data-ttu-id="646ed-113">**Stav testu protokoly:** tento protokol můžete použít k zobrazení problémů zjištěných váš test stavu, jako je počet instancí ve vašem fondu back-end, které nejsou přijímání požadavků z nástroje pro vyrovnávání zatížení z důvodu selhání kontroly stavu.</span><span class="sxs-lookup"><span data-stu-id="646ed-113">**Health probe logs:** You can use this log to view problems detected by your health probe, such as the number of instances in your backend-pool that are not receiving requests from the load balancer because of health probe failures.</span></span> <span data-ttu-id="646ed-114">Tento protokol je zapsán do, když dojde ke změně v stav testu.</span><span class="sxs-lookup"><span data-stu-id="646ed-114">This log is written to when there is a change in the health probe status.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="646ed-115">Analýza protokolu aktuálně funguje pouze pro internetové nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="646ed-115">Log analytics currently works only for Internet facing load balancers.</span></span> <span data-ttu-id="646ed-116">Protokoly jsou dostupné pouze pro prostředky nasazené v modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="646ed-116">Logs are only available for resources deployed in the Resource Manager deployment model.</span></span> <span data-ttu-id="646ed-117">Protokoly nelze použít pro prostředky v modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="646ed-117">You cannot use logs for resources in the classic deployment model.</span></span> <span data-ttu-id="646ed-118">Další informace o modelech nasazení najdete v tématu [nasazení Resource Manager principy a nasazení classic](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="646ed-118">For more information about the deployment models, see [Understanding Resource Manager deployment and classic deployment](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>

## <a name="enable-logging"></a><span data-ttu-id="646ed-119">Povolit protokolování</span><span class="sxs-lookup"><span data-stu-id="646ed-119">Enable logging</span></span>

<span data-ttu-id="646ed-120">Pro každý prostředek Resource Manager je automaticky povolené protokolování auditu.</span><span class="sxs-lookup"><span data-stu-id="646ed-120">Audit logging is automatically enabled for every Resource Manager resource.</span></span> <span data-ttu-id="646ed-121">Budete muset povolit události a stav testu protokolování spustit shromažďování dat, které jsou k dispozici prostřednictvím tyto protokoly.</span><span class="sxs-lookup"><span data-stu-id="646ed-121">You need to enable event and health probe logging to start collecting the data available through those logs.</span></span> <span data-ttu-id="646ed-122">Pomocí následujících kroků povolte protokolování.</span><span class="sxs-lookup"><span data-stu-id="646ed-122">Use the following steps to enable logging.</span></span>

<span data-ttu-id="646ed-123">Přihlaste se do [portál Azure](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="646ed-123">Sign-in to the [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="646ed-124">Pokud ještě nemáte nástroj pro vyrovnávání zatížení [vytvořit nástroj pro vyrovnávání zatížení](load-balancer-get-started-internet-arm-ps.md) než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="646ed-124">If you don't already have a load balancer, [create a load balancer](load-balancer-get-started-internet-arm-ps.md) before you continue.</span></span>

1. <span data-ttu-id="646ed-125">Na portálu, klikněte na tlačítko **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="646ed-125">In the portal, click **Browse**.</span></span>
2. <span data-ttu-id="646ed-126">Vyberte **nástroje pro vyrovnávání zatížení**.</span><span class="sxs-lookup"><span data-stu-id="646ed-126">Select **Load Balancers**.</span></span>

    ![portál – nástroj na Vyrovnávání zatížení](./media/load-balancer-monitor-log/load-balancer-browse.png)

3. <span data-ttu-id="646ed-128">Vyberte existující pro vyrovnávání zatížení >> **všechna nastavení**.</span><span class="sxs-lookup"><span data-stu-id="646ed-128">Select an existing load balancer >> **All Settings**.</span></span>
4. <span data-ttu-id="646ed-129">Na pravé straně dialogového okna pod názvem služby Vyrovnávání zatížení, přejděte na **monitorování**, klikněte na tlačítko **diagnostiky**.</span><span class="sxs-lookup"><span data-stu-id="646ed-129">On the right side of the dialog under the name of the load balancer, scroll to **Monitoring**, click **Diagnostics**.</span></span>

    ![portál – nastavení vyrovnávání zátěže](./media/load-balancer-monitor-log/load-balancer-settings.png)

5. <span data-ttu-id="646ed-131">V **diagnostiky** podokně v části **stav**, vyberte **na**.</span><span class="sxs-lookup"><span data-stu-id="646ed-131">In the **Diagnostics** pane, under **Status**, select **On**.</span></span>
6. <span data-ttu-id="646ed-132">Klikněte na tlačítko **účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="646ed-132">Click **Storage Account**.</span></span>
7. <span data-ttu-id="646ed-133">V části **protokoly**, vybrat existující účet úložiště, nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="646ed-133">Under **LOGS**, select an existing storage account, or create a new one.</span></span> <span data-ttu-id="646ed-134">Posuvníkem určit, kolik dní, po který data událostí se uloží v protokolech událostí.</span><span class="sxs-lookup"><span data-stu-id="646ed-134">Use the slider to determine how many days worth of event data will be stored in the event logs.</span></span> 
8. <span data-ttu-id="646ed-135">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="646ed-135">Click **Save**.</span></span>

    ![Portál – protokolů diagnostiky](./media/load-balancer-monitor-log/load-balancer-diagnostics.png)

> [!NOTE]
> <span data-ttu-id="646ed-137">Protokoly auditu nevyžadují, aby účet samostatného úložiště.</span><span class="sxs-lookup"><span data-stu-id="646ed-137">Audit logs do not require a separate storage account.</span></span> <span data-ttu-id="646ed-138">Použití úložiště pro události a stav testu protokolování bude platit poplatky služby.</span><span class="sxs-lookup"><span data-stu-id="646ed-138">The use of storage for event and health probe logging will incur service charges.</span></span>

## <a name="audit-log"></a><span data-ttu-id="646ed-139">Protokol auditování</span><span class="sxs-lookup"><span data-stu-id="646ed-139">Audit log</span></span>

<span data-ttu-id="646ed-140">Ve výchozím nastavení je generování protokolu auditu.</span><span class="sxs-lookup"><span data-stu-id="646ed-140">The audit log is generated by default.</span></span> <span data-ttu-id="646ed-141">Protokoly se zachovají 90 dní v úložišti Azure a protokoly událostí.</span><span class="sxs-lookup"><span data-stu-id="646ed-141">The logs are preserved for 90 days in Azure's Event Logs store.</span></span> <span data-ttu-id="646ed-142">Další informace o tyto protokoly načtením [zobrazení událostí a protokolů auditování](../monitoring-and-diagnostics/insights-debugging-with-events.md) článku.</span><span class="sxs-lookup"><span data-stu-id="646ed-142">Learn more about these logs by reading the [View events and audit logs](../monitoring-and-diagnostics/insights-debugging-with-events.md) article.</span></span>

## <a name="alert-event-log"></a><span data-ttu-id="646ed-143">Upozornění v protokolu událostí</span><span class="sxs-lookup"><span data-stu-id="646ed-143">Alert event log</span></span>

<span data-ttu-id="646ed-144">Tento protokol je generovaný pouze v případě, že jste povolili na na základě nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="646ed-144">This log is only generated if you've enabled it on a per load balancer basis.</span></span> <span data-ttu-id="646ed-145">Události jsou zaznamenána ve formátu JSON a uložený v účtu storage, kterou jste zadali při jste povolili protokolování.</span><span class="sxs-lookup"><span data-stu-id="646ed-145">The events are logged in JSON format and stored in the storage account you specified when you enabled the logging.</span></span> <span data-ttu-id="646ed-146">Následuje příklad události.</span><span class="sxs-lookup"><span data-stu-id="646ed-146">The following is an example of an event.</span></span>

```json
{
    "time": "2016-01-26T10:37:46.6024215Z",
    "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
    "category": "LoadBalancerAlertEvent",
    "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
    "operationName": "LoadBalancerProbeHealthStatus",
    "properties": {
        "eventName": "Resource Limits Hit",
        "eventDescription": "Ports exhausted",
        "eventProperties": {
            "public ip address": "40.117.227.32"
        }
    }
}
```

<span data-ttu-id="646ed-147">JSON výstup ukazuje *eventname* vlastnost, která popíše z důvodu nástroj pro vyrovnávání zatížení vytvořit výstrahu.</span><span class="sxs-lookup"><span data-stu-id="646ed-147">The JSON output shows the *eventname* property which will describe the reason for the load balancer created an alert.</span></span> <span data-ttu-id="646ed-148">V takovém případě se výstrahy generované z důvodu vyčerpání port TCP způsobené zdrojové IP NAT omezení (překládat pomocí SNAT).</span><span class="sxs-lookup"><span data-stu-id="646ed-148">In this case, the alert generated was due to TCP port exhaustion caused by source IP NAT limits (SNAT).</span></span>

## <a name="health-probe-log"></a><span data-ttu-id="646ed-149">Stav testu protokolu</span><span class="sxs-lookup"><span data-stu-id="646ed-149">Health probe log</span></span>

<span data-ttu-id="646ed-150">Tento protokol je generovaný pouze v případě, že jste povolili na za zatížení vyrovnávání základ popsaných výše.</span><span class="sxs-lookup"><span data-stu-id="646ed-150">This log is only generated if you've enabled it on a per load balancer basis as detailed above.</span></span> <span data-ttu-id="646ed-151">Data je uložený v účtu úložiště, kterou jste zadali při jste povolili protokolování.</span><span class="sxs-lookup"><span data-stu-id="646ed-151">The data is stored in the storage account you specified when you enabled the logging.</span></span> <span data-ttu-id="646ed-152">Je-li vytvořit kontejner s názvem "insights loadbalancerprobehealthstatus protokoly, a se protokolují tato data:</span><span class="sxs-lookup"><span data-stu-id="646ed-152">A container named 'insights-logs-loadbalancerprobehealthstatus' is created and the following data is logged:</span></span>

```json
{
    "records":[
    {
        "time": "2016-01-26T10:37:46.6024215Z",
        "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
        "category": "LoadBalancerProbeHealthStatus",
        "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
        "operationName": "LoadBalancerProbeHealthStatus",
        "properties": {
            "publicIpAddress": "40.83.190.158",
            "port": "81",
            "totalDipCount": 2,
            "dipDownCount": 1,
            "healthPercentage": 50.000000
        }
    },
    {
        "time": "2016-01-26T10:37:46.6024215Z",
        "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
        "category": "LoadBalancerProbeHealthStatus",
        "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
        "operationName": "LoadBalancerProbeHealthStatus",
        "properties": {
            "publicIpAddress": "40.83.190.158",
            "port": "81",
            "totalDipCount": 2,
            "dipDownCount": 0,
            "healthPercentage": 100.000000
        }
    }]
}
```

<span data-ttu-id="646ed-153">Výstup JSON obsahuje v poli vlastnosti základní informace o stav testu.</span><span class="sxs-lookup"><span data-stu-id="646ed-153">The JSON output shows in the properties field the basic information for the probe health status.</span></span> <span data-ttu-id="646ed-154">*DipDownCount* vlastnost se zobrazuje celkový počet instancí v back-end, které nejsou přijímá síťový provoz z důvodu selhání testu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="646ed-154">The *dipDownCount* property shows the total number of instances on the back-end which are not receiving network traffic due to failed probe responses.</span></span>

## <a name="view-and-analyze-the-audit-log"></a><span data-ttu-id="646ed-155">Zobrazit a analyzovat protokol auditu</span><span class="sxs-lookup"><span data-stu-id="646ed-155">View and analyze the audit log</span></span>

<span data-ttu-id="646ed-156">Můžete zobrazit a analyzovat data protokolu auditu pomocí kteréhokoli z následujících metod:</span><span class="sxs-lookup"><span data-stu-id="646ed-156">You can view and analyze audit log data using any of the following methods:</span></span>

* <span data-ttu-id="646ed-157">**Nástroje Azure:** načítat informace z protokolů auditu prostřednictvím prostředí Azure PowerShell, rozhraní příkazového řádku Azure (CLI), REST API služby Azure nebo portálu Azure preview.</span><span class="sxs-lookup"><span data-stu-id="646ed-157">**Azure tools:** Retrieve information from the audit logs through Azure PowerShell, the Azure Command Line Interface (CLI), the Azure REST API, or the Azure preview portal.</span></span> <span data-ttu-id="646ed-158">Podrobné pokyny pro jednotlivé metody jsou podrobně popsané na [auditovat operace s Resource Managerem](../azure-resource-manager/resource-group-audit.md) článku.</span><span class="sxs-lookup"><span data-stu-id="646ed-158">Step-by-step instructions for each method are detailed in the [Audit operations with Resource Manager](../azure-resource-manager/resource-group-audit.md) article.</span></span>
* <span data-ttu-id="646ed-159">**Power BI:** Pokud již nemáte [Power BI](https://powerbi.microsoft.com/pricing) účet, můžete zkusit je zdarma.</span><span class="sxs-lookup"><span data-stu-id="646ed-159">**Power BI:** If you do not already have a [Power BI](https://powerbi.microsoft.com/pricing) account, you can try it for free.</span></span> <span data-ttu-id="646ed-160">Pomocí [obsahu protokoly auditu Azure pack pro Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs), můžete analyzovat svá data pomocí předem nakonfigurovaných řídicí panely, nebo můžete přizpůsobit zobrazení podle svých potřeb.</span><span class="sxs-lookup"><span data-stu-id="646ed-160">Using the [Azure Audit Logs content pack for Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs), you can analyze your data with pre-configured dashboards, or you can customize views to suit your requirements.</span></span>

## <a name="view-and-analyze-the-health-probe-and-event-log"></a><span data-ttu-id="646ed-161">Zobrazení a analýza protokolů událostí a test stavu</span><span class="sxs-lookup"><span data-stu-id="646ed-161">View and analyze the health probe and event log</span></span>

<span data-ttu-id="646ed-162">Budete muset připojit k účtu úložiště a načítat položky protokolu JSON pro kontrolu protokolů událostí a stavu.</span><span class="sxs-lookup"><span data-stu-id="646ed-162">You need to connect to your storage account and retrieve the JSON log entries for event and health probe logs.</span></span> <span data-ttu-id="646ed-163">Jakmile si stáhnout soubory JSON, můžete je převést na sdílený svazek clusteru a zobrazení v aplikaci Excel, PowerBI nebo jakýkoli jiný nástroj pro vizualizaci dat.</span><span class="sxs-lookup"><span data-stu-id="646ed-163">Once you download the JSON files, you can convert them to CSV and view in Excel, PowerBI, or any other data visualization tool.</span></span>

> [!TIP]
> <span data-ttu-id="646ed-164">Pokud jste obeznámeni s Visual Studio a základní koncepty změna hodnoty konstanty a proměnné v jazyce C#, můžete použít [protokolu nástroje Převaděč](https://github.com/Azure-Samples/networking-dotnet-log-converter) dostupné z Githubu.</span><span class="sxs-lookup"><span data-stu-id="646ed-164">If you are familiar with Visual Studio and basic concepts of changing values for constants and variables in C#, you can use the [log converter tools](https://github.com/Azure-Samples/networking-dotnet-log-converter) available from GitHub.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="646ed-165">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="646ed-165">Additional resources</span></span>

* <span data-ttu-id="646ed-166">[Vaše protokoly auditu Azure s Power BI vizualizovat](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) příspěvku na blogu.</span><span class="sxs-lookup"><span data-stu-id="646ed-166">[Visualize your Azure Audit Logs with Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blog post.</span></span>
* <span data-ttu-id="646ed-167">[Zobrazení a analýza protokolů auditu Azure v Power BI a další](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) příspěvku na blogu.</span><span class="sxs-lookup"><span data-stu-id="646ed-167">[View and analyze Azure Audit Logs in Power BI and more](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blog post.</span></span>

## <a name="next-steps"></a><span data-ttu-id="646ed-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="646ed-168">Next steps</span></span>

[<span data-ttu-id="646ed-169">Porozumění testům nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="646ed-169">Understand load balancer probes</span></span>](load-balancer-custom-probe-overview.md)
