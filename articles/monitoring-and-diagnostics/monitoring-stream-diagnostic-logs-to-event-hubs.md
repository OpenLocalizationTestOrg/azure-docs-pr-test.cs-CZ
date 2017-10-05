---
title: "Stream Azure diagnostické protokoly na Namespace centra událostí | Microsoft Docs"
description: "Zjistěte, jak k vysílání datového proudu Azure diagnostické protokoly na obor názvů služby Event Hubs."
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
ms.openlocfilehash: 01ba8ddfcf90e1368ac147296fd180f99420d96f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="stream-azure-diagnostic-logs-to-an-event-hubs-namespace"></a><span data-ttu-id="f6a68-103">Stream Azure diagnostické protokoly na Namespace centra událostí</span><span class="sxs-lookup"><span data-stu-id="f6a68-103">Stream Azure Diagnostic Logs to an Event Hubs Namespace</span></span>
<span data-ttu-id="f6a68-104">**[Azure diagnostické protokoly](monitoring-overview-of-diagnostic-logs.md)**  Streamovat skoro v reálném čase pro všechny aplikace pomocí předdefinované možnosti "Export do služby Event Hubs" na portálu nebo povolením ID Service Bus pravidla v nastavení diagnostiky prostřednictvím rutin prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="f6a68-104">**[Azure diagnostic logs](monitoring-overview-of-diagnostic-logs.md)** can be streamed in near real time to any application using the built-in “Export to Event Hubs” option in the Portal, or by enabling the Service Bus Rule ID in a diagnostic setting via the Azure PowerShell Cmdlets or Azure CLI.</span></span>

## <a name="what-you-can-do-with-diagnostics-logs-and-event-hubs"></a><span data-ttu-id="f6a68-105">Co můžete dělat s protokolů diagnostiky a Event Hubs</span><span class="sxs-lookup"><span data-stu-id="f6a68-105">What you can do with diagnostics logs and Event Hubs</span></span>
<span data-ttu-id="f6a68-106">Můžete použít k diagnostickým protokolům streamování schopností několika způsoby:</span><span class="sxs-lookup"><span data-stu-id="f6a68-106">Here are just a few ways you might use the streaming capability for Diagnostic Logs:</span></span>

* <span data-ttu-id="f6a68-107">**Stream protokoluje události do 3. stran protokolování a telemetrie systémy** – v čase, streamování Event Hubs se stane mechanismus kanálem k diagnostickým protokolům v jiných systémů Siem a řešení pro analýzu protokolu.</span><span class="sxs-lookup"><span data-stu-id="f6a68-107">**Stream logs to 3rd party logging and telemetry systems** – Over time, Event Hubs streaming will become the mechanism to pipe your diagnostic logs in to third-party SIEMs and log analytics solutions.</span></span>
* <span data-ttu-id="f6a68-108">**Zobrazit stav služby streamování "aktivní cesta" dat do PowerBI** – pomocí služby Event Hubs, Stream Analytics a PowerBI, můžete snadno transformovat data diagnostiky v poblíž přehledy v reálném čase na služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="f6a68-108">**View service health by streaming “hot path” data to PowerBI** – Using Event Hubs, Stream Analytics, and PowerBI, you can easily transform your diagnostics data in to near real-time insights on your Azure services.</span></span> <span data-ttu-id="f6a68-109">[V tomto článku dokumentace podává přehled o tom, jak nastavit službu Event Hubs, zpracování dat pomocí služby Stream Analytics a použít PowerBI jako výstup](../stream-analytics/stream-analytics-power-bi-dashboard.md).</span><span class="sxs-lookup"><span data-stu-id="f6a68-109">[This documentation article gives a great overview of how to set up Event Hubs, process data with Stream Analytics, and use PowerBI as an output](../stream-analytics/stream-analytics-power-bi-dashboard.md).</span></span> <span data-ttu-id="f6a68-110">Zde je několik tipů pro získání nastavení diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="f6a68-110">Here are a few tips for getting set up with diagnostic logs:</span></span>
  
  * <span data-ttu-id="f6a68-111">Centra událostí pro kategorii diagnostické protokoly se vytvoří automaticky, když zaškrtnutí políčka na portálu nebo povolit pomocí prostředí PowerShell, kterou chcete vybrat centra událostí v oboru názvů s názvem, který začíná **insights -**.</span><span class="sxs-lookup"><span data-stu-id="f6a68-111">An event hub for a category of diagnostic logs is created automatically when you check the option in the portal or enable it through PowerShell, so you want to select the event hub in the namespace with the name that starts with **insights-**.</span></span>
  * <span data-ttu-id="f6a68-112">Následující kód SQL je ukázkový dotaz služby Stream Analytics, který můžete použít k analýze všechna data protokolu v do PowerBI tabulky:</span><span class="sxs-lookup"><span data-stu-id="f6a68-112">The following SQL code is a sample Stream Analytics query that you can use to parse all the log data in to a PowerBI table:</span></span>

    ```sql
    SELECT
    records.ArrayValue.[Properties you want to track]
    INTO
    [OutputSourceName – the PowerBI source]
    FROM
    [InputSourceName] AS e
    CROSS APPLY GetArrayElements(e.records) AS records
    ```

* <span data-ttu-id="f6a68-113">**Vytvoření vlastní telemetrii a protokolování platformy** – Pokud už máte uživatelské telemetrie platformy nebo jsou jenom přemýšlíte o vytváření jeden vysoce škálovatelné, publikování a odběru na povaze služby Event Hubs umožňuje flexibilně ingestování diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="f6a68-113">**Build a custom telemetry and logging platform** – If you already have a custom-built telemetry platform or are just thinking about building one, the highly scalable publish-subscribe nature of Event Hubs allows you to flexibly ingest diagnostic logs.</span></span> <span data-ttu-id="f6a68-114">[Naleznete v Průvodci Dana Rosanova pomocí služby Event Hubs telemetrie platformy globálním měřítku zde](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).</span><span class="sxs-lookup"><span data-stu-id="f6a68-114">[See Dan Rosanova’s guide to using Event Hubs in a global scale telemetry platform here](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).</span></span>

## <a name="enable-streaming-of-diagnostic-logs"></a><span data-ttu-id="f6a68-115">Povolení diagnostických protokolů streamování</span><span class="sxs-lookup"><span data-stu-id="f6a68-115">Enable streaming of diagnostic logs</span></span>
<span data-ttu-id="f6a68-116">Můžete povolit vysílání datového proudu diagnostické protokoly prostřednictvím kódu programu, prostřednictvím portálu nebo pomocí [rozhraní REST API Azure monitorování](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings).</span><span class="sxs-lookup"><span data-stu-id="f6a68-116">You can enable streaming of diagnostic logs programmatically, via the portal, or using the [Azure Monitor REST APIs](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings).</span></span> <span data-ttu-id="f6a68-117">V obou případech vytvoříte nastavení diagnostiky v němž jsou uvedeny na obor názvů služby Event Hubs a protokolu kategorií a metriky, které se mají posílat do oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="f6a68-117">Either way, you create a diagnostic setting in which you specify an Event Hubs namespace and the log categories and metrics you want to send in to the namespace.</span></span> <span data-ttu-id="f6a68-118">Centra událostí je vytvořen v oboru názvů pro každou kategorii protokolu, které povolíte.</span><span class="sxs-lookup"><span data-stu-id="f6a68-118">An event hub is created in the namespace for each log category you enable.</span></span> <span data-ttu-id="f6a68-119">Diagnostika **kategorie protokolu** je typ protokolu, který může shromažďovat prostředku.</span><span class="sxs-lookup"><span data-stu-id="f6a68-119">A diagnostic **log category** is a type of log that a resource may collect.</span></span>

> [!WARNING]
> <span data-ttu-id="f6a68-120">Povolení a vysílání datového proudu diagnostických protokolů z výpočetní prostředky (například virtuální počítače nebo Service Fabric) [vyžaduje jinou sadu kroků](../event-hubs/event-hubs-streaming-azure-diags-data.md).</span><span class="sxs-lookup"><span data-stu-id="f6a68-120">Enabling and streaming diagnostic logs from Compute resources (for example, VMs or Service Fabric) [requires a different set of steps](../event-hubs/event-hubs-streaming-azure-diags-data.md).</span></span>
> 
> 

<span data-ttu-id="f6a68-121">Obor názvů Service Bus nebo Event Hubs nemusí být ve stejném předplatném jako prostředek emitování protokoly tak dlouho, dokud uživatel, který konfiguruje nastavení, má odpovídající přístup RBAC do oba odběry.</span><span class="sxs-lookup"><span data-stu-id="f6a68-121">The Service Bus or Event Hubs namespace does not have to be in the same subscription as the resource emitting logs as long as the user who configures the setting has appropriate RBAC access to both subscriptions.</span></span>

## <a name="stream-diagnostic-logs-using-the-portal"></a><span data-ttu-id="f6a68-122">Diagnostické protokoly datového proudu pomocí portálu</span><span class="sxs-lookup"><span data-stu-id="f6a68-122">Stream diagnostic logs using the portal</span></span>
1. <span data-ttu-id="f6a68-123">Na portálu, přejděte do monitorování Azure a klikněte na **nastavení diagnostiky**</span><span class="sxs-lookup"><span data-stu-id="f6a68-123">In the portal, navigate to Azure Monitor and click on **Diagnostic Settings**</span></span>

    ![Monitorování části monitorování Azure](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-blade.png)

2. <span data-ttu-id="f6a68-125">Volitelně můžete seznam filtrovat podle skupiny prostředků nebo typ prostředku, a potom klikněte na prostředek, pro kterou chcete provést nastavení diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="f6a68-125">Optionally filter the list by resource group or resource type, then click on the resource for which you would like to set a diagnostic setting.</span></span>

3. <span data-ttu-id="f6a68-126">Pokud neexistuje žádné nastavení na prostředku jste vybrali, zobrazí se výzva k vytvoření nastavení.</span><span class="sxs-lookup"><span data-stu-id="f6a68-126">If no settings exist on the resource you have selected, you are prompted to create a setting.</span></span> <span data-ttu-id="f6a68-127">Klikněte na tlačítko "Zapnout diagnostiky."</span><span class="sxs-lookup"><span data-stu-id="f6a68-127">Click "Turn on diagnostics."</span></span>

   ![Přidat nastavení diagnostiky - žádná existující nastavení](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-none.png)

   <span data-ttu-id="f6a68-129">Pokud máte stávající nastavení na prostředek, zobrazí se seznam nastavení, které jsou již nakonfigurována na tomto prostředku.</span><span class="sxs-lookup"><span data-stu-id="f6a68-129">If there are existing settings on the resource, you will see a list of settings already configured on this resource.</span></span> <span data-ttu-id="f6a68-130">Klikněte na tlačítko "Přidat nastavení diagnostiky."</span><span class="sxs-lookup"><span data-stu-id="f6a68-130">Click "Add diagnostic setting."</span></span>

   ![Přidat nastavení diagnostiky - stávající nastavení](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-multiple.png)

3. <span data-ttu-id="f6a68-132">Zadejte název nastavení a zaškrtněte políčko pro **datový proud do centra událostí**, pak vyberte obor názvů Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="f6a68-132">Give your setting a name and check the box for **Stream to an event hub**, then select an Event Hubs namespace.</span></span>
   
   ![Přidat nastavení diagnostiky - stávající nastavení](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-configure.png)
    
   <span data-ttu-id="f6a68-134">Obor názvů vybrané bude kde vytvoření (pokud to vaše první čas diagnostické protokoly streamování) nebo prostřednictvím datového proudu do centra událostí (Pokud již existují prostředky, které jsou streamování této kategorie protokolu na tento obor názvů), a definuje zásady oprávnění, která má streamování mechanismus.</span><span class="sxs-lookup"><span data-stu-id="f6a68-134">The namespace selected will be where the event hub is created (if this is your first time streaming diagnostic logs) or streamed to (if there are already resources that are streaming that log category to this namespace), and the policy defines the permissions that the streaming mechanism has.</span></span> <span data-ttu-id="f6a68-135">V současné době streamování do centra událostí vyžadují oprávnění spravovat, odeslání a naslouchání.</span><span class="sxs-lookup"><span data-stu-id="f6a68-135">Today, streaming to an event hub requires Manage, Send, and Listen permissions.</span></span> <span data-ttu-id="f6a68-136">Můžete vytvářet nebo upravovat Event Hubs obor názvů sdílených zásad přístupu na portálu na kartě Konfigurace oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="f6a68-136">You can create or modify Event Hubs namespace shared access policies in the portal under the Configure tab for your namespace.</span></span> <span data-ttu-id="f6a68-137">Pokud chcete aktualizovat jednu z těchto nastavení diagnostiky, klient musí mít oprávnění ListKey na autorizační pravidlo Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="f6a68-137">To update one of these diagnostic settings, the client must have the ListKey permission on the Event Hubs authorization rule.</span></span>

4. <span data-ttu-id="f6a68-138">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f6a68-138">Click **Save**.</span></span>

<span data-ttu-id="f6a68-139">Po chvíli se nové nastavení se zobrazí v seznamu nastavení pro tento prostředek a diagnostické protokoly jsou datového proudu k tomuto účtu úložiště, také se vygeneruje nová data události.</span><span class="sxs-lookup"><span data-stu-id="f6a68-139">After a few moments, the new setting appears in your list of settings for this resource, and diagnostic logs are streamed to that storage account as soon as new event data is generated.</span></span>

### <a name="via-powershell-cmdlets"></a><span data-ttu-id="f6a68-140">Pomocí rutin prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="f6a68-140">Via PowerShell Cmdlets</span></span>
<span data-ttu-id="f6a68-141">Povolit vysílání datového proudu prostřednictvím [rutin prostředí Azure PowerShell](insights-powershell-samples.md), můžete použít `Set-AzureRmDiagnosticSetting` rutiny s těmito parametry:</span><span class="sxs-lookup"><span data-stu-id="f6a68-141">To enable streaming via the [Azure PowerShell Cmdlets](insights-powershell-samples.md), you can use the `Set-AzureRmDiagnosticSetting` cmdlet with these parameters:</span></span>

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource ID] -ServiceBusRuleId [your Service Bus rule ID] -Enabled $true
```

<span data-ttu-id="f6a68-142">ID pravidla Service Bus je řetězec s Tento formát: `{Service Bus resource ID}/authorizationrules/{key name}`, například `/subscriptions/{subscription ID}/resourceGroups/Default-ServiceBus-WestUS/providers/Microsoft.ServiceBus/namespaces/{Service Bus namespace}/authorizationrules/RootManageSharedAccessKey`.</span><span class="sxs-lookup"><span data-stu-id="f6a68-142">The Service Bus Rule ID is a string with this format: `{Service Bus resource ID}/authorizationrules/{key name}`, for example, `/subscriptions/{subscription ID}/resourceGroups/Default-ServiceBus-WestUS/providers/Microsoft.ServiceBus/namespaces/{Service Bus namespace}/authorizationrules/RootManageSharedAccessKey`.</span></span>

### <a name="via-azure-cli"></a><span data-ttu-id="f6a68-143">Prostřednictvím rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="f6a68-143">Via Azure CLI</span></span>
<span data-ttu-id="f6a68-144">Povolit vysílání datového proudu prostřednictvím [rozhraní příkazového řádku Azure](insights-cli-samples.md), můžete použít `insights diagnostic set` příkaz takto:</span><span class="sxs-lookup"><span data-stu-id="f6a68-144">To enable streaming via the [Azure CLI](insights-cli-samples.md), you can use the `insights diagnostic set` command like this:</span></span>

```azurecli
azure insights diagnostic set --resourceId <resourceID> --serviceBusRuleId <serviceBusRuleID> --enabled true
```

<span data-ttu-id="f6a68-145">Jak je popsáno pro rutinu prostředí PowerShell, použijte stejný formát pro ID pravidla Service Bus.</span><span class="sxs-lookup"><span data-stu-id="f6a68-145">Use the same format for Service Bus Rule ID as explained for the PowerShell Cmdlet.</span></span>

## <a name="how-do-i-consume-the-log-data-from-event-hubs"></a><span data-ttu-id="f6a68-146">Způsob, jakým využívají data protokolu ze služby Event Hubs?</span><span class="sxs-lookup"><span data-stu-id="f6a68-146">How do I consume the log data from Event Hubs?</span></span>
<span data-ttu-id="f6a68-147">Zde je ukázka výstupní data ze služby Event Hubs:</span><span class="sxs-lookup"><span data-stu-id="f6a68-147">Here is sample output data from Event Hubs:</span></span>

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

| <span data-ttu-id="f6a68-148">Název elementu</span><span class="sxs-lookup"><span data-stu-id="f6a68-148">Element Name</span></span> | <span data-ttu-id="f6a68-149">Popis</span><span class="sxs-lookup"><span data-stu-id="f6a68-149">Description</span></span> |
| --- | --- |
| <span data-ttu-id="f6a68-150">záznamy</span><span class="sxs-lookup"><span data-stu-id="f6a68-150">records</span></span> |<span data-ttu-id="f6a68-151">Pole všechny protokolu události v této datové části.</span><span class="sxs-lookup"><span data-stu-id="f6a68-151">An array of all log events in this payload.</span></span> |
| <span data-ttu-id="f6a68-152">time</span><span class="sxs-lookup"><span data-stu-id="f6a68-152">time</span></span> |<span data-ttu-id="f6a68-153">Čas, kdy došlo k události.</span><span class="sxs-lookup"><span data-stu-id="f6a68-153">Time at which the event occurred.</span></span> |
| <span data-ttu-id="f6a68-154">category</span><span class="sxs-lookup"><span data-stu-id="f6a68-154">category</span></span> |<span data-ttu-id="f6a68-155">Kategorie protokolu pro tuto událost.</span><span class="sxs-lookup"><span data-stu-id="f6a68-155">Log category for this event.</span></span> |
| <span data-ttu-id="f6a68-156">resourceId</span><span class="sxs-lookup"><span data-stu-id="f6a68-156">resourceId</span></span> |<span data-ttu-id="f6a68-157">ID prostředku prostředku, který vytvořil tuto událost.</span><span class="sxs-lookup"><span data-stu-id="f6a68-157">Resource ID of the resource that generated this event.</span></span> |
| <span data-ttu-id="f6a68-158">operationName</span><span class="sxs-lookup"><span data-stu-id="f6a68-158">operationName</span></span> |<span data-ttu-id="f6a68-159">Název operace.</span><span class="sxs-lookup"><span data-stu-id="f6a68-159">Name of the operation.</span></span> |
| <span data-ttu-id="f6a68-160">úroveň</span><span class="sxs-lookup"><span data-stu-id="f6a68-160">level</span></span> |<span data-ttu-id="f6a68-161">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="f6a68-161">Optional.</span></span> <span data-ttu-id="f6a68-162">Určuje úroveň protokolu událostí.</span><span class="sxs-lookup"><span data-stu-id="f6a68-162">Indicates the log event level.</span></span> |
| <span data-ttu-id="f6a68-163">properties</span><span class="sxs-lookup"><span data-stu-id="f6a68-163">properties</span></span> |<span data-ttu-id="f6a68-164">Vlastnosti události.</span><span class="sxs-lookup"><span data-stu-id="f6a68-164">Properties of the event.</span></span> |

<span data-ttu-id="f6a68-165">Můžete zobrazit seznam všech poskytovatelů prostředků, které podporují streamování Event Hubs [zde](monitoring-overview-of-diagnostic-logs.md).</span><span class="sxs-lookup"><span data-stu-id="f6a68-165">You can view a list of all resource providers that support streaming to Event Hubs [here](monitoring-overview-of-diagnostic-logs.md).</span></span>

## <a name="stream-data-from-compute-resources"></a><span data-ttu-id="f6a68-166">Datový proud dat z výpočetní prostředky</span><span class="sxs-lookup"><span data-stu-id="f6a68-166">Stream data from Compute resources</span></span>
<span data-ttu-id="f6a68-167">Můžete také stream diagnostických protokolů z výpočetní prostředky pomocí agenta Windows Azure Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="f6a68-167">You can also stream diagnostic logs from Compute resources using the Windows Azure Diagnostics agent.</span></span> <span data-ttu-id="f6a68-168">[Najdete v článku](../event-hubs/event-hubs-streaming-azure-diags-data.md) jak k tomuto nastavení.</span><span class="sxs-lookup"><span data-stu-id="f6a68-168">[See this article](../event-hubs/event-hubs-streaming-azure-diags-data.md) for how to set that up.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f6a68-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f6a68-169">Next steps</span></span>
* [<span data-ttu-id="f6a68-170">Další informace o diagnostických protokolů Azure.</span><span class="sxs-lookup"><span data-stu-id="f6a68-170">Read more about Azure Diagnostic Logs</span></span>](monitoring-overview-of-diagnostic-logs.md)
* [<span data-ttu-id="f6a68-171">Začínáme s Event Hubs</span><span class="sxs-lookup"><span data-stu-id="f6a68-171">Get started with Event Hubs</span></span>](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

