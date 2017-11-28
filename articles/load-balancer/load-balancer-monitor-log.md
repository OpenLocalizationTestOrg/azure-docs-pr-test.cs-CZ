---
title: "aaaMonitor operace, události a čítače pro nástroj pro vyrovnávání zatížení | Microsoft Docs"
description: "Zjistěte, jak tooenable výstrahy událostí a sběru dat stavu stav protokolování pro nástroj pro vyrovnávání zatížení Azure"
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
ms.openlocfilehash: ac53c2254e06cad780ad6144c5c30f0085d12576
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-for-azure-load-balancer"></a><span data-ttu-id="493fb-103">Log Analytics pro Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="493fb-103">Log analytics for Azure Load Balancer</span></span>

<span data-ttu-id="493fb-104">Můžete použít různé typy protokolů v Azure toomanage a řešení potíží s nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="493fb-104">You can use different types of logs in Azure toomanage and troubleshoot load balancers.</span></span> <span data-ttu-id="493fb-105">Některé z těchto protokolů jsou přístupné prostřednictvím portálu hello.</span><span class="sxs-lookup"><span data-stu-id="493fb-105">Some of these logs can be accessed through hello portal.</span></span> <span data-ttu-id="493fb-106">Všechny protokoly můžete extrahovat z Azure blob storage a zobrazit v různých nástrojů, jako je například aplikace Excel a PowerBI.</span><span class="sxs-lookup"><span data-stu-id="493fb-106">All logs can be extracted from Azure blob storage, and viewed in different tools, such as Excel and PowerBI.</span></span> <span data-ttu-id="493fb-107">Další informace o různých typech hello protokolů v následujícím seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="493fb-107">You can learn more about hello different types of logs from hello list below.</span></span>

* <span data-ttu-id="493fb-108">**Protokoly auditu:** můžete použít [protokoly auditu Azure](../monitoring-and-diagnostics/insights-debugging-with-events.md) (dříve označované jako operační protokoly) tooview všechny operace se odeslaná tooyour předplatná Azure a jejich stav.</span><span class="sxs-lookup"><span data-stu-id="493fb-108">**Audit logs:** You can use [Azure Audit Logs](../monitoring-and-diagnostics/insights-debugging-with-events.md) (formerly known as Operational Logs) tooview all operations being submitted tooyour Azure subscription(s), and their status.</span></span> <span data-ttu-id="493fb-109">Protokoly auditu jsou ve výchozím nastavení povolené a lze zobrazit v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="493fb-109">Audit logs are enabled by default, and can be viewed in hello Azure portal.</span></span>
* <span data-ttu-id="493fb-110">**Výstrahy protokoly událostí:** tento protokol tooview výstrahy rasied můžete použít nástroj pro vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="493fb-110">**Alert event logs:** You can use this log tooview alerts rasied by hello load balancer.</span></span> <span data-ttu-id="493fb-111">Stav Hello nástroje pro vyrovnávání zatížení hello shromažďovaných každých pět minut.</span><span class="sxs-lookup"><span data-stu-id="493fb-111">hello status for hello load balancer is collected every five minutes.</span></span> <span data-ttu-id="493fb-112">Tento protokol je zapsán pouze pokud je vyvolána o událost výstrahy nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="493fb-112">This log is only written if a load balancer alert event is raised.</span></span>
* <span data-ttu-id="493fb-113">**Stav testu protokoly:** můžete použít tento protokol tooview problémů zjištěných váš test stavu, jako je například hello počet instancí ve vašem fondu back-end, které nejsou přijímání požadavků z nástroje pro vyrovnávání zatížení hello z důvodu selhání kontroly stavu.</span><span class="sxs-lookup"><span data-stu-id="493fb-113">**Health probe logs:** You can use this log tooview problems detected by your health probe, such as hello number of instances in your backend-pool that are not receiving requests from hello load balancer because of health probe failures.</span></span> <span data-ttu-id="493fb-114">Tento protokol je zapsán toowhen dojde ke změně v hello stav testu.</span><span class="sxs-lookup"><span data-stu-id="493fb-114">This log is written toowhen there is a change in hello health probe status.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="493fb-115">Analýza protokolu aktuálně funguje pouze pro internetové nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="493fb-115">Log analytics currently works only for Internet facing load balancers.</span></span> <span data-ttu-id="493fb-116">Protokoly jsou dostupné pouze pro prostředky nasazené v modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="493fb-116">Logs are only available for resources deployed in hello Resource Manager deployment model.</span></span> <span data-ttu-id="493fb-117">Protokoly nelze použít pro prostředky v modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="493fb-117">You cannot use logs for resources in hello classic deployment model.</span></span> <span data-ttu-id="493fb-118">Další informace o modelech nasazení hello najdete v tématu [nasazení Resource Manager principy a nasazení classic](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="493fb-118">For more information about hello deployment models, see [Understanding Resource Manager deployment and classic deployment](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>

## <a name="enable-logging"></a><span data-ttu-id="493fb-119">Povolit protokolování</span><span class="sxs-lookup"><span data-stu-id="493fb-119">Enable logging</span></span>

<span data-ttu-id="493fb-120">Pro každý prostředek Resource Manager je automaticky povolené protokolování auditu.</span><span class="sxs-lookup"><span data-stu-id="493fb-120">Audit logging is automatically enabled for every Resource Manager resource.</span></span> <span data-ttu-id="493fb-121">Potřebujete tooenable události a stav testu protokolování toostart shromažďování dat hello k dispozici prostřednictvím tyto protokoly.</span><span class="sxs-lookup"><span data-stu-id="493fb-121">You need tooenable event and health probe logging toostart collecting hello data available through those logs.</span></span> <span data-ttu-id="493fb-122">Pomocí následujících kroků tooenable protokolování hello.</span><span class="sxs-lookup"><span data-stu-id="493fb-122">Use hello following steps tooenable logging.</span></span>

<span data-ttu-id="493fb-123">Přihlášení toohello [portál Azure](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="493fb-123">Sign-in toohello [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="493fb-124">Pokud ještě nemáte nástroj pro vyrovnávání zatížení [vytvořit nástroj pro vyrovnávání zatížení](load-balancer-get-started-internet-arm-ps.md) než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="493fb-124">If you don't already have a load balancer, [create a load balancer](load-balancer-get-started-internet-arm-ps.md) before you continue.</span></span>

1. <span data-ttu-id="493fb-125">Hello portálu, klikněte na tlačítko **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="493fb-125">In hello portal, click **Browse**.</span></span>
2. <span data-ttu-id="493fb-126">Vyberte **nástroje pro vyrovnávání zatížení**.</span><span class="sxs-lookup"><span data-stu-id="493fb-126">Select **Load Balancers**.</span></span>

    ![portál – nástroj na Vyrovnávání zatížení](./media/load-balancer-monitor-log/load-balancer-browse.png)

3. <span data-ttu-id="493fb-128">Vyberte existující pro vyrovnávání zatížení >> **všechna nastavení**.</span><span class="sxs-lookup"><span data-stu-id="493fb-128">Select an existing load balancer >> **All Settings**.</span></span>
4. <span data-ttu-id="493fb-129">Na pravé straně hello dialogového okna hello pod názvem hello nástroje pro vyrovnávání zatížení hello, posuňte příliš**monitorování**, klikněte na tlačítko **diagnostiky**.</span><span class="sxs-lookup"><span data-stu-id="493fb-129">On hello right side of hello dialog under hello name of hello load balancer, scroll too**Monitoring**, click **Diagnostics**.</span></span>

    ![portál – nastavení vyrovnávání zátěže](./media/load-balancer-monitor-log/load-balancer-settings.png)

5. <span data-ttu-id="493fb-131">V hello **diagnostiky** podokně v části **stav**, vyberte **na**.</span><span class="sxs-lookup"><span data-stu-id="493fb-131">In hello **Diagnostics** pane, under **Status**, select **On**.</span></span>
6. <span data-ttu-id="493fb-132">Klikněte na tlačítko **účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="493fb-132">Click **Storage Account**.</span></span>
7. <span data-ttu-id="493fb-133">V části **protokoly**, vybrat existující účet úložiště, nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="493fb-133">Under **LOGS**, select an existing storage account, or create a new one.</span></span> <span data-ttu-id="493fb-134">Pomocí posuvníku toodetermine hello počet dní, za data událostí se uloží v protokolech událostí hello.</span><span class="sxs-lookup"><span data-stu-id="493fb-134">Use hello slider toodetermine how many days worth of event data will be stored in hello event logs.</span></span> 
8. <span data-ttu-id="493fb-135">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="493fb-135">Click **Save**.</span></span>

    ![Portál – protokolů diagnostiky](./media/load-balancer-monitor-log/load-balancer-diagnostics.png)

> [!NOTE]
> <span data-ttu-id="493fb-137">Protokoly auditu nevyžadují, aby účet samostatného úložiště.</span><span class="sxs-lookup"><span data-stu-id="493fb-137">Audit logs do not require a separate storage account.</span></span> <span data-ttu-id="493fb-138">Hello použití úložiště pro události a stav testu protokolování bude platit poplatky služby.</span><span class="sxs-lookup"><span data-stu-id="493fb-138">hello use of storage for event and health probe logging will incur service charges.</span></span>

## <a name="audit-log"></a><span data-ttu-id="493fb-139">Protokol auditování</span><span class="sxs-lookup"><span data-stu-id="493fb-139">Audit log</span></span>

<span data-ttu-id="493fb-140">ve výchozím nastavení je generování Hello protokolu auditu.</span><span class="sxs-lookup"><span data-stu-id="493fb-140">hello audit log is generated by default.</span></span> <span data-ttu-id="493fb-141">protokoly Hello se zachovají 90 dní v úložišti Azure a protokoly událostí.</span><span class="sxs-lookup"><span data-stu-id="493fb-141">hello logs are preserved for 90 days in Azure's Event Logs store.</span></span> <span data-ttu-id="493fb-142">Další informace o tyto protokoly načtením hello [zobrazení událostí a protokolů auditování](../monitoring-and-diagnostics/insights-debugging-with-events.md) článku.</span><span class="sxs-lookup"><span data-stu-id="493fb-142">Learn more about these logs by reading hello [View events and audit logs](../monitoring-and-diagnostics/insights-debugging-with-events.md) article.</span></span>

## <a name="alert-event-log"></a><span data-ttu-id="493fb-143">Upozornění v protokolu událostí</span><span class="sxs-lookup"><span data-stu-id="493fb-143">Alert event log</span></span>

<span data-ttu-id="493fb-144">Tento protokol je generovaný pouze v případě, že jste povolili na na základě nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="493fb-144">This log is only generated if you've enabled it on a per load balancer basis.</span></span> <span data-ttu-id="493fb-145">Hello události se zaznamenávají ve formátu JSON a uložený v účtu storage hello, že jste zadali, pokud jste povolili protokolování hello.</span><span class="sxs-lookup"><span data-stu-id="493fb-145">hello events are logged in JSON format and stored in hello storage account you specified when you enabled hello logging.</span></span> <span data-ttu-id="493fb-146">Hello tady je příklad události.</span><span class="sxs-lookup"><span data-stu-id="493fb-146">hello following is an example of an event.</span></span>

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

<span data-ttu-id="493fb-147">výstup ukazuje hello Hello JSON *eventname* vlastnost, která popíše hello důvod pro vyrovnávání zatížení hello vytvořit výstrahu.</span><span class="sxs-lookup"><span data-stu-id="493fb-147">hello JSON output shows hello *eventname* property which will describe hello reason for hello load balancer created an alert.</span></span> <span data-ttu-id="493fb-148">V takovém případě hello výstraha generována se z důvodu tooTCP port vyčerpání způsobené zdroj, který omezuje IP NAT (překládat pomocí SNAT).</span><span class="sxs-lookup"><span data-stu-id="493fb-148">In this case, hello alert generated was due tooTCP port exhaustion caused by source IP NAT limits (SNAT).</span></span>

## <a name="health-probe-log"></a><span data-ttu-id="493fb-149">Stav testu protokolu</span><span class="sxs-lookup"><span data-stu-id="493fb-149">Health probe log</span></span>

<span data-ttu-id="493fb-150">Tento protokol je generovaný pouze v případě, že jste povolili na za zatížení vyrovnávání základ popsaných výše.</span><span class="sxs-lookup"><span data-stu-id="493fb-150">This log is only generated if you've enabled it on a per load balancer basis as detailed above.</span></span> <span data-ttu-id="493fb-151">Hello data je uložený v účtu úložiště hello, že jste zadali, pokud jste povolili protokolování hello.</span><span class="sxs-lookup"><span data-stu-id="493fb-151">hello data is stored in hello storage account you specified when you enabled hello logging.</span></span> <span data-ttu-id="493fb-152">Je-li vytvořit kontejner s názvem "insights loadbalancerprobehealthstatus protokoly, a je zaznamenána hello následující data:</span><span class="sxs-lookup"><span data-stu-id="493fb-152">A container named 'insights-logs-loadbalancerprobehealthstatus' is created and hello following data is logged:</span></span>

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

<span data-ttu-id="493fb-153">výstup JSON Hello zobrazuje hello vlastnosti pole hello základní informace o hello test stavu.</span><span class="sxs-lookup"><span data-stu-id="493fb-153">hello JSON output shows in hello properties field hello basic information for hello probe health status.</span></span> <span data-ttu-id="493fb-154">Hello *dipDownCount* vlastnost zobrazuje hello celkový počet instancí v hello back-end, které nejsou přijímá síťový provoz z důvodu toofailed testu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="493fb-154">hello *dipDownCount* property shows hello total number of instances on hello back-end which are not receiving network traffic due toofailed probe responses.</span></span>

## <a name="view-and-analyze-hello-audit-log"></a><span data-ttu-id="493fb-155">Zobrazení a analýza protokolů auditu hello</span><span class="sxs-lookup"><span data-stu-id="493fb-155">View and analyze hello audit log</span></span>

<span data-ttu-id="493fb-156">Můžete zobrazit a analyzovat data protokolu auditu pomocí kteréhokoli hello následující metody:</span><span class="sxs-lookup"><span data-stu-id="493fb-156">You can view and analyze audit log data using any of hello following methods:</span></span>

* <span data-ttu-id="493fb-157">**Nástroje Azure:** načíst informace z protokolů auditu hello pomocí Azure Powershellu hello Azure rozhraní příkazového řádku (CLI), hello REST API služby Azure, nebo hello portál Azure preview.</span><span class="sxs-lookup"><span data-stu-id="493fb-157">**Azure tools:** Retrieve information from hello audit logs through Azure PowerShell, hello Azure Command Line Interface (CLI), hello Azure REST API, or hello Azure preview portal.</span></span> <span data-ttu-id="493fb-158">Podrobné pokyny pro jednotlivé metody jsou podrobně popsané na hello [auditovat operace s Resource Managerem](../azure-resource-manager/resource-group-audit.md) článku.</span><span class="sxs-lookup"><span data-stu-id="493fb-158">Step-by-step instructions for each method are detailed in hello [Audit operations with Resource Manager](../azure-resource-manager/resource-group-audit.md) article.</span></span>
* <span data-ttu-id="493fb-159">**Power BI:** Pokud již nemáte [Power BI](https://powerbi.microsoft.com/pricing) účet, můžete zkusit je zdarma.</span><span class="sxs-lookup"><span data-stu-id="493fb-159">**Power BI:** If you do not already have a [Power BI](https://powerbi.microsoft.com/pricing) account, you can try it for free.</span></span> <span data-ttu-id="493fb-160">Pomocí hello [obsahu protokoly auditu Azure pack pro Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs), můžete analyzovat svá data pomocí předem nakonfigurovaných řídicí panely, nebo můžete přizpůsobit zobrazení toosuit vašim požadavkům.</span><span class="sxs-lookup"><span data-stu-id="493fb-160">Using hello [Azure Audit Logs content pack for Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs), you can analyze your data with pre-configured dashboards, or you can customize views toosuit your requirements.</span></span>

## <a name="view-and-analyze-hello-health-probe-and-event-log"></a><span data-ttu-id="493fb-161">Zobrazení a analýza protokolů událostí a test stavu hello</span><span class="sxs-lookup"><span data-stu-id="493fb-161">View and analyze hello health probe and event log</span></span>

<span data-ttu-id="493fb-162">Potřebujete úložiště, tooyour tooconnect účtu a načítat položky protokolu hello JSON pro kontrolu protokolů událostí a stavu.</span><span class="sxs-lookup"><span data-stu-id="493fb-162">You need tooconnect tooyour storage account and retrieve hello JSON log entries for event and health probe logs.</span></span> <span data-ttu-id="493fb-163">Jakmile si stáhnout soubory hello JSON, můžete je převést tooCSV a zobrazení v aplikaci Excel, PowerBI nebo jakýkoli jiný nástroj pro vizualizaci dat.</span><span class="sxs-lookup"><span data-stu-id="493fb-163">Once you download hello JSON files, you can convert them tooCSV and view in Excel, PowerBI, or any other data visualization tool.</span></span>

> [!TIP]
> <span data-ttu-id="493fb-164">Pokud jste obeznámeni s Visual Studio a základní koncepty změna hodnoty konstanty a proměnné v jazyce C#, můžete použít hello [protokolu nástroje Převaděč](https://github.com/Azure-Samples/networking-dotnet-log-converter) dostupné z Githubu.</span><span class="sxs-lookup"><span data-stu-id="493fb-164">If you are familiar with Visual Studio and basic concepts of changing values for constants and variables in C#, you can use hello [log converter tools](https://github.com/Azure-Samples/networking-dotnet-log-converter) available from GitHub.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="493fb-165">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="493fb-165">Additional resources</span></span>

* <span data-ttu-id="493fb-166">[Vaše protokoly auditu Azure s Power BI vizualizovat](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) příspěvku na blogu.</span><span class="sxs-lookup"><span data-stu-id="493fb-166">[Visualize your Azure Audit Logs with Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blog post.</span></span>
* <span data-ttu-id="493fb-167">[Zobrazení a analýza protokolů auditu Azure v Power BI a další](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) příspěvku na blogu.</span><span class="sxs-lookup"><span data-stu-id="493fb-167">[View and analyze Azure Audit Logs in Power BI and more](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blog post.</span></span>

## <a name="next-steps"></a><span data-ttu-id="493fb-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="493fb-168">Next steps</span></span>

[<span data-ttu-id="493fb-169">Porozumění testům nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="493fb-169">Understand load balancer probes</span></span>](load-balancer-custom-probe-overview.md)
