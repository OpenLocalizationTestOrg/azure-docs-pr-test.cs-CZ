---
title: "tooan aaaStream diagnostických protokolů Azure Event Hubs Namespace | Microsoft Docs"
description: "Zjistěte, jak toostream Azure diagnostické protokoly služby Event Hubs tooan oboru názvů."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 42bc4845-c564-4568-b72d-0614591ebd80
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: johnkem
ms.openlocfilehash: 00092ea8f3fe4fa1476e3a697bf1e8645dd21e6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="stream-azure-diagnostic-logs-tooan-event-hubs-namespace"></a><span data-ttu-id="c0f3b-103">Datový proud diagnostických protokolů Azure tooan Namespace centra událostí</span><span class="sxs-lookup"><span data-stu-id="c0f3b-103">Stream Azure Diagnostic Logs tooan Event Hubs Namespace</span></span>
<span data-ttu-id="c0f3b-104">**[Azure diagnostické protokoly](monitoring-overview-of-diagnostic-logs.md)**  Streamovat v téměř v reálném čase tooany aplikace pomocí hello předdefinované "Export tooEvent centra" možnost v hello portál nebo povolením hello Service Bus ID pravidla v nastavení diagnostiky prostřednictvím hello prostředí Azure PowerShell Rutiny nebo Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="c0f3b-104">**[Azure diagnostic logs](monitoring-overview-of-diagnostic-logs.md)** can be streamed in near real time tooany application using hello built-in “Export tooEvent Hubs” option in hello Portal, or by enabling hello Service Bus Rule ID in a diagnostic setting via hello Azure PowerShell Cmdlets or Azure CLI.</span></span>

## <a name="what-you-can-do-with-diagnostics-logs-and-event-hubs"></a><span data-ttu-id="c0f3b-105">Co můžete dělat s protokolů diagnostiky a Event Hubs</span><span class="sxs-lookup"><span data-stu-id="c0f3b-105">What you can do with diagnostics logs and Event Hubs</span></span>
<span data-ttu-id="c0f3b-106">Můžete použít hello streamování schopností diagnostické protokoly několika způsoby:</span><span class="sxs-lookup"><span data-stu-id="c0f3b-106">Here are just a few ways you might use hello streaming capability for Diagnostic Logs:</span></span>

* <span data-ttu-id="c0f3b-107">**Datový proud protokolů too3rd strany protokolování a telemetrie systémy** – v čase, streamování Event Hubs bude toopipe mechanismus hello k diagnostickým protokolům v toothird výrobců systémů Siem a řešení pro analýzu protokolu.</span><span class="sxs-lookup"><span data-stu-id="c0f3b-107">**Stream logs too3rd party logging and telemetry systems** – Over time, Event Hubs streaming will become hello mechanism toopipe your diagnostic logs in toothird-party SIEMs and log analytics solutions.</span></span>
* <span data-ttu-id="c0f3b-108">**Zobrazit stav služby streamování "aktivní cesta" data tooPowerBI** – pomocí služby Event Hubs, Stream Analytics a PowerBI, můžete snadno transformovat data diagnostiky v toonear přehledy v reálném čase na služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="c0f3b-108">**View service health by streaming “hot path” data tooPowerBI** – Using Event Hubs, Stream Analytics, and PowerBI, you can easily transform your diagnostics data in toonear real-time insights on your Azure services.</span></span> <span data-ttu-id="c0f3b-109">[V tomto článku dokumentace podává přehled jak tooset se službou Event Hubs, zpracování dat pomocí služby Stream Analytics a použít jako výstup PowerBI](../stream-analytics/stream-analytics-power-bi-dashboard.md).</span><span class="sxs-lookup"><span data-stu-id="c0f3b-109">[This documentation article gives a great overview of how tooset up Event Hubs, process data with Stream Analytics, and use PowerBI as an output](../stream-analytics/stream-analytics-power-bi-dashboard.md).</span></span> <span data-ttu-id="c0f3b-110">Zde je několik tipů pro získání nastavení diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="c0f3b-110">Here are a few tips for getting set up with diagnostic logs:</span></span>
  
  * <span data-ttu-id="c0f3b-111">Centra událostí pro kategorii diagnostické protokoly se vytvoří automaticky při ověření hello možnost hello portálu nebo povolit pomocí prostředí PowerShell, takže chcete Centrum událostí hello tooselect hello oboru názvů s hello název, který začíná **insights**.</span><span class="sxs-lookup"><span data-stu-id="c0f3b-111">An event hub for a category of diagnostic logs is created automatically when you check hello option in hello portal or enable it through PowerShell, so you want tooselect hello event hub in hello namespace with hello name that starts with **insights-**.</span></span>
  * <span data-ttu-id="c0f3b-112">Následující kód SQL Hello je ukázka Stream Analytics dotazu, které můžete tooparse všechna data protokolu hello v tabulce PowerBI tooa:</span><span class="sxs-lookup"><span data-stu-id="c0f3b-112">hello following SQL code is a sample Stream Analytics query that you can use tooparse all hello log data in tooa PowerBI table:</span></span>

    ```sql
    SELECT
    records.ArrayValue.[Properties you want tootrack]
    INTO
    [OutputSourceName – hello PowerBI source]
    FROM
    [InputSourceName] AS e
    CROSS APPLY GetArrayElements(e.records) AS records
    ```

* <span data-ttu-id="c0f3b-113">**Vytvoření vlastní telemetrii a protokolování platformy** – Pokud už máte uživatelské telemetrie platformy nebo jsou právě přemýšlíte o vytváření jeden hello vysoce škálovatelné publikování a odběru povaha Event Hubs vám umožní tooflexibly ingestování diagnostiky protokoly.</span><span class="sxs-lookup"><span data-stu-id="c0f3b-113">**Build a custom telemetry and logging platform** – If you already have a custom-built telemetry platform or are just thinking about building one, hello highly scalable publish-subscribe nature of Event Hubs allows you tooflexibly ingest diagnostic logs.</span></span> <span data-ttu-id="c0f3b-114">[V tématu Dana Rosanova Průvodce toousing Event Hubs telemetrie platformy globálním měřítku zde](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).</span><span class="sxs-lookup"><span data-stu-id="c0f3b-114">[See Dan Rosanova’s guide toousing Event Hubs in a global scale telemetry platform here](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).</span></span>

## <a name="enable-streaming-of-diagnostic-logs"></a><span data-ttu-id="c0f3b-115">Povolení diagnostických protokolů streamování</span><span class="sxs-lookup"><span data-stu-id="c0f3b-115">Enable streaming of diagnostic logs</span></span>
<span data-ttu-id="c0f3b-116">Můžete povolit vysílání datového proudu diagnostické protokoly prostřednictvím kódu programu, prostřednictvím portálu hello, nebo pomocí hello [rozhraní REST API Azure monitorování](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings).</span><span class="sxs-lookup"><span data-stu-id="c0f3b-116">You can enable streaming of diagnostic logs programmatically, via hello portal, or using hello [Azure Monitor REST APIs](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings).</span></span> <span data-ttu-id="c0f3b-117">V obou případech vytvoříte nastavení diagnostiky ve kterém můžete zadat na obor názvů služby Event Hubs a hello protokolu kategorií a metriky, které chcete toosend v oboru názvů toohello.</span><span class="sxs-lookup"><span data-stu-id="c0f3b-117">Either way, you create a diagnostic setting in which you specify an Event Hubs namespace and hello log categories and metrics you want toosend in toohello namespace.</span></span> <span data-ttu-id="c0f3b-118">Centra událostí je vytvořen v oboru názvů hello pro každou kategorii protokolu, které povolíte.</span><span class="sxs-lookup"><span data-stu-id="c0f3b-118">An event hub is created in hello namespace for each log category you enable.</span></span> <span data-ttu-id="c0f3b-119">Diagnostika **kategorie protokolu** je typ protokolu, který může shromažďovat prostředku.</span><span class="sxs-lookup"><span data-stu-id="c0f3b-119">A diagnostic **log category** is a type of log that a resource may collect.</span></span>

> [!WARNING]
> <span data-ttu-id="c0f3b-120">Povolení a vysílání datového proudu diagnostických protokolů z výpočetní prostředky (například virtuální počítače nebo Service Fabric) [vyžaduje jinou sadu kroků](../event-hubs/event-hubs-streaming-azure-diags-data.md).</span><span class="sxs-lookup"><span data-stu-id="c0f3b-120">Enabling and streaming diagnostic logs from Compute resources (for example, VMs or Service Fabric) [requires a different set of steps](../event-hubs/event-hubs-streaming-azure-diags-data.md).</span></span>
> 
> 

<span data-ttu-id="c0f3b-121">Hello Service Bus nebo Event Hubs obor názvů nemá toobe v hello stejného předplatného jako prostředek hello emitování protokoly tak dlouho, dokud hello uživatel, který konfiguruje nastavení hello má příslušné předplatné tooboth přístupu RBAC.</span><span class="sxs-lookup"><span data-stu-id="c0f3b-121">hello Service Bus or Event Hubs namespace does not have toobe in hello same subscription as hello resource emitting logs as long as hello user who configures hello setting has appropriate RBAC access tooboth subscriptions.</span></span>

## <a name="stream-diagnostic-logs-using-hello-portal"></a><span data-ttu-id="c0f3b-122">Datový proud diagnostické protokoly portálu hello</span><span class="sxs-lookup"><span data-stu-id="c0f3b-122">Stream diagnostic logs using hello portal</span></span>
1. <span data-ttu-id="c0f3b-123">Hello portálu, přejděte tooAzure monitorování a klikněte na **nastavení diagnostiky**</span><span class="sxs-lookup"><span data-stu-id="c0f3b-123">In hello portal, navigate tooAzure Monitor and click on **Diagnostic Settings**</span></span>

    ![Monitorování části monitorování Azure](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-blade.png)

2. <span data-ttu-id="c0f3b-125">Volitelně můžete filtrovat seznam hello skupinu prostředků nebo typ prostředku, a potom klikněte na hello prostředků, pro kterou chcete tooset nastavení diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="c0f3b-125">Optionally filter hello list by resource group or resource type, then click on hello resource for which you would like tooset a diagnostic setting.</span></span>

3. <span data-ttu-id="c0f3b-126">Pokud neexistuje žádné nastavení na hello prostředku, který jste zvolili, jste výzvami toocreate nastavení.</span><span class="sxs-lookup"><span data-stu-id="c0f3b-126">If no settings exist on hello resource you have selected, you are prompted toocreate a setting.</span></span> <span data-ttu-id="c0f3b-127">Klikněte na tlačítko "Zapnout diagnostiky."</span><span class="sxs-lookup"><span data-stu-id="c0f3b-127">Click "Turn on diagnostics."</span></span>

   ![Přidat nastavení diagnostiky - žádná existující nastavení](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-none.png)

   <span data-ttu-id="c0f3b-129">Pokud máte stávající nastavení na hello prostředku, zobrazí se seznam nastavení, které jsou již nakonfigurována na tomto prostředku.</span><span class="sxs-lookup"><span data-stu-id="c0f3b-129">If there are existing settings on hello resource, you will see a list of settings already configured on this resource.</span></span> <span data-ttu-id="c0f3b-130">Klikněte na tlačítko "Přidat nastavení diagnostiky."</span><span class="sxs-lookup"><span data-stu-id="c0f3b-130">Click "Add diagnostic setting."</span></span>

   ![Přidat nastavení diagnostiky - stávající nastavení](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-multiple.png)

3. <span data-ttu-id="c0f3b-132">Zadejte název nastavení a zaškrtněte políčko hello pro **centra událostí tooan datového proudu**, pak vyberte obor názvů Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="c0f3b-132">Give your setting a name and check hello box for **Stream tooan event hub**, then select an Event Hubs namespace.</span></span>
   
   ![Přidat nastavení diagnostiky - stávající nastavení](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-configure.png)
    
   <span data-ttu-id="c0f3b-134">Hello vybraný obor názvů bude kde je hello centra událostí vytvořit (Pokud je to poprvé diagnostické protokoly streamování) nebo prostřednictvím datového proudu příliš (Pokud již existují prostředky, které jsou streamování tento obor názvů protokolu kategorie toothis), a definuje zásady hello hello oprávnění, která má streamování mechanismus hello.</span><span class="sxs-lookup"><span data-stu-id="c0f3b-134">hello namespace selected will be where hello event hub is created (if this is your first time streaming diagnostic logs) or streamed too(if there are already resources that are streaming that log category toothis namespace), and hello policy defines hello permissions that hello streaming mechanism has.</span></span> <span data-ttu-id="c0f3b-135">V současné době streamování tooan centra událostí vyžadují oprávnění spravovat, odeslání a naslouchání.</span><span class="sxs-lookup"><span data-stu-id="c0f3b-135">Today, streaming tooan event hub requires Manage, Send, and Listen permissions.</span></span> <span data-ttu-id="c0f3b-136">Můžete vytvářet nebo upravovat zásady přístupu obor názvů sdílených Event Hubs portálu hello kartě hello konfigurace oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="c0f3b-136">You can create or modify Event Hubs namespace shared access policies in hello portal under hello Configure tab for your namespace.</span></span> <span data-ttu-id="c0f3b-137">tooupdate jednu z těchto nastavení diagnostiky, hello klienta musí mít oprávnění ListKey hello na hello Event Hubs autorizační pravidlo.</span><span class="sxs-lookup"><span data-stu-id="c0f3b-137">tooupdate one of these diagnostic settings, hello client must have hello ListKey permission on hello Event Hubs authorization rule.</span></span>

4. <span data-ttu-id="c0f3b-138">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c0f3b-138">Click **Save**.</span></span>

<span data-ttu-id="c0f3b-139">Po chvíli se hello nové nastavení se zobrazí v seznamu nastavení pro tento prostředek a diagnostické protokoly jsou streamování toothat účet úložiště, také se vygeneruje nová data události.</span><span class="sxs-lookup"><span data-stu-id="c0f3b-139">After a few moments, hello new setting appears in your list of settings for this resource, and diagnostic logs are streamed toothat storage account as soon as new event data is generated.</span></span>

### <a name="via-powershell-cmdlets"></a><span data-ttu-id="c0f3b-140">Pomocí rutin prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="c0f3b-140">Via PowerShell Cmdlets</span></span>
<span data-ttu-id="c0f3b-141">tooenable vysílání datového proudu prostřednictvím hello [rutin prostředí Azure PowerShell](insights-powershell-samples.md), můžete použít hello `Set-AzureRmDiagnosticSetting` rutiny s těmito parametry:</span><span class="sxs-lookup"><span data-stu-id="c0f3b-141">tooenable streaming via hello [Azure PowerShell Cmdlets](insights-powershell-samples.md), you can use hello `Set-AzureRmDiagnosticSetting` cmdlet with these parameters:</span></span>

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource ID] -ServiceBusRuleId [your Service Bus rule ID] -Enabled $true
```

<span data-ttu-id="c0f3b-142">Hello ID pravidla Service Bus je řetězec s Tento formát: `{Service Bus resource ID}/authorizationrules/{key name}`, například `/subscriptions/{subscription ID}/resourceGroups/Default-ServiceBus-WestUS/providers/Microsoft.ServiceBus/namespaces/{Service Bus namespace}/authorizationrules/RootManageSharedAccessKey`.</span><span class="sxs-lookup"><span data-stu-id="c0f3b-142">hello Service Bus Rule ID is a string with this format: `{Service Bus resource ID}/authorizationrules/{key name}`, for example, `/subscriptions/{subscription ID}/resourceGroups/Default-ServiceBus-WestUS/providers/Microsoft.ServiceBus/namespaces/{Service Bus namespace}/authorizationrules/RootManageSharedAccessKey`.</span></span>

### <a name="via-azure-cli"></a><span data-ttu-id="c0f3b-143">Prostřednictvím rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="c0f3b-143">Via Azure CLI</span></span>
<span data-ttu-id="c0f3b-144">tooenable vysílání datového proudu prostřednictvím hello [rozhraní příkazového řádku Azure](insights-cli-samples.md), můžete použít hello `insights diagnostic set` příkaz takto:</span><span class="sxs-lookup"><span data-stu-id="c0f3b-144">tooenable streaming via hello [Azure CLI](insights-cli-samples.md), you can use hello `insights diagnostic set` command like this:</span></span>

```azurecli
azure insights diagnostic set --resourceId <resourceID> --serviceBusRuleId <serviceBusRuleID> --enabled true
```

<span data-ttu-id="c0f3b-145">Použijte hello stejný formát pro Service Bus pravidlo ID, jak je popsáno pro hello rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c0f3b-145">Use hello same format for Service Bus Rule ID as explained for hello PowerShell Cmdlet.</span></span>

## <a name="how-do-i-consume-hello-log-data-from-event-hubs"></a><span data-ttu-id="c0f3b-146">Způsob, jakým využívají data protokolu hello ze služby Event Hubs?</span><span class="sxs-lookup"><span data-stu-id="c0f3b-146">How do I consume hello log data from Event Hubs?</span></span>
<span data-ttu-id="c0f3b-147">Zde je ukázka výstupní data ze služby Event Hubs:</span><span class="sxs-lookup"><span data-stu-id="c0f3b-147">Here is sample output data from Event Hubs:</span></span>

```json
{
    "records": [
        {
            "time": "2016-07-15T18:00:22.6235064Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330013509921957/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Error",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T17:58:55.048482Z",
                "endTime": "2016-07-15T18:00:22.4109204Z",
                "status": "Failed",
                "code": "BadGateway",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330013509921957",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "29a9862f-969b-4c70-90c4-dfbdc814e413",
                    "clientTrackingId": "08587330013509921958"
                }
            }
        },
        {
            "time": "2016-07-15T18:01:15.7532989Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330012106702630/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionStarted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T18:01:15.5828115Z",
                "status": "Running",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330012106702630",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "042fb72c-7bd4-439e-89eb-3cf4409d429e",
                    "clientTrackingId": "08587330012106702632"
                }
            }
        }
    ]
}
```

| <span data-ttu-id="c0f3b-148">Název elementu</span><span class="sxs-lookup"><span data-stu-id="c0f3b-148">Element Name</span></span> | <span data-ttu-id="c0f3b-149">Popis</span><span class="sxs-lookup"><span data-stu-id="c0f3b-149">Description</span></span> |
| --- | --- |
| <span data-ttu-id="c0f3b-150">záznamy</span><span class="sxs-lookup"><span data-stu-id="c0f3b-150">records</span></span> |<span data-ttu-id="c0f3b-151">Pole všechny protokolu události v této datové části.</span><span class="sxs-lookup"><span data-stu-id="c0f3b-151">An array of all log events in this payload.</span></span> |
| <span data-ttu-id="c0f3b-152">time</span><span class="sxs-lookup"><span data-stu-id="c0f3b-152">time</span></span> |<span data-ttu-id="c0f3b-153">Čas, kdy hello k události došlo.</span><span class="sxs-lookup"><span data-stu-id="c0f3b-153">Time at which hello event occurred.</span></span> |
| <span data-ttu-id="c0f3b-154">category</span><span class="sxs-lookup"><span data-stu-id="c0f3b-154">category</span></span> |<span data-ttu-id="c0f3b-155">Kategorie protokolu pro tuto událost.</span><span class="sxs-lookup"><span data-stu-id="c0f3b-155">Log category for this event.</span></span> |
| <span data-ttu-id="c0f3b-156">resourceId</span><span class="sxs-lookup"><span data-stu-id="c0f3b-156">resourceId</span></span> |<span data-ttu-id="c0f3b-157">ID prostředku hello prostředku, který vytvořil tuto událost.</span><span class="sxs-lookup"><span data-stu-id="c0f3b-157">Resource ID of hello resource that generated this event.</span></span> |
| <span data-ttu-id="c0f3b-158">operationName</span><span class="sxs-lookup"><span data-stu-id="c0f3b-158">operationName</span></span> |<span data-ttu-id="c0f3b-159">Název operace hello.</span><span class="sxs-lookup"><span data-stu-id="c0f3b-159">Name of hello operation.</span></span> |
| <span data-ttu-id="c0f3b-160">úroveň</span><span class="sxs-lookup"><span data-stu-id="c0f3b-160">level</span></span> |<span data-ttu-id="c0f3b-161">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="c0f3b-161">Optional.</span></span> <span data-ttu-id="c0f3b-162">Určuje úroveň hello protokolu událostí.</span><span class="sxs-lookup"><span data-stu-id="c0f3b-162">Indicates hello log event level.</span></span> |
| <span data-ttu-id="c0f3b-163">properties</span><span class="sxs-lookup"><span data-stu-id="c0f3b-163">properties</span></span> |<span data-ttu-id="c0f3b-164">Vlastnosti události hello.</span><span class="sxs-lookup"><span data-stu-id="c0f3b-164">Properties of hello event.</span></span> |

<span data-ttu-id="c0f3b-165">Můžete zobrazit seznam všech poskytovatelů prostředků podporující streamování centra tooEvent [zde](monitoring-overview-of-diagnostic-logs.md).</span><span class="sxs-lookup"><span data-stu-id="c0f3b-165">You can view a list of all resource providers that support streaming tooEvent Hubs [here](monitoring-overview-of-diagnostic-logs.md).</span></span>

## <a name="stream-data-from-compute-resources"></a><span data-ttu-id="c0f3b-166">Datový proud dat z výpočetní prostředky</span><span class="sxs-lookup"><span data-stu-id="c0f3b-166">Stream data from Compute resources</span></span>
<span data-ttu-id="c0f3b-167">Můžete také stream diagnostických protokolů z výpočetní prostředky pomocí agenta Windows Azure Diagnostics hello.</span><span class="sxs-lookup"><span data-stu-id="c0f3b-167">You can also stream diagnostic logs from Compute resources using hello Windows Azure Diagnostics agent.</span></span> <span data-ttu-id="c0f3b-168">[Najdete v článku](../event-hubs/event-hubs-streaming-azure-diags-data.md) jak tooset této nahoru.</span><span class="sxs-lookup"><span data-stu-id="c0f3b-168">[See this article](../event-hubs/event-hubs-streaming-azure-diags-data.md) for how tooset that up.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c0f3b-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c0f3b-169">Next steps</span></span>
* [<span data-ttu-id="c0f3b-170">Další informace o diagnostických protokolů Azure.</span><span class="sxs-lookup"><span data-stu-id="c0f3b-170">Read more about Azure Diagnostic Logs</span></span>](monitoring-overview-of-diagnostic-logs.md)
* [<span data-ttu-id="c0f3b-171">Začínáme s Event Hubs</span><span class="sxs-lookup"><span data-stu-id="c0f3b-171">Get started with Event Hubs</span></span>](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

