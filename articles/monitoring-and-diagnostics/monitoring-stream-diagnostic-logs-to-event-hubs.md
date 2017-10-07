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
# <a name="stream-azure-diagnostic-logs-tooan-event-hubs-namespace"></a>Datový proud diagnostických protokolů Azure tooan Namespace centra událostí
**[Azure diagnostické protokoly](monitoring-overview-of-diagnostic-logs.md)**  Streamovat v téměř v reálném čase tooany aplikace pomocí hello předdefinované "Export tooEvent centra" možnost v hello portál nebo povolením hello Service Bus ID pravidla v nastavení diagnostiky prostřednictvím hello prostředí Azure PowerShell Rutiny nebo Azure CLI.

## <a name="what-you-can-do-with-diagnostics-logs-and-event-hubs"></a>Co můžete dělat s protokolů diagnostiky a Event Hubs
Můžete použít hello streamování schopností diagnostické protokoly několika způsoby:

* **Datový proud protokolů too3rd strany protokolování a telemetrie systémy** – v čase, streamování Event Hubs bude toopipe mechanismus hello k diagnostickým protokolům v toothird výrobců systémů Siem a řešení pro analýzu protokolu.
* **Zobrazit stav služby streamování "aktivní cesta" data tooPowerBI** – pomocí služby Event Hubs, Stream Analytics a PowerBI, můžete snadno transformovat data diagnostiky v toonear přehledy v reálném čase na služeb Azure. [V tomto článku dokumentace podává přehled jak tooset se službou Event Hubs, zpracování dat pomocí služby Stream Analytics a použít jako výstup PowerBI](../stream-analytics/stream-analytics-power-bi-dashboard.md). Zde je několik tipů pro získání nastavení diagnostické protokoly.
  
  * Centra událostí pro kategorii diagnostické protokoly se vytvoří automaticky při ověření hello možnost hello portálu nebo povolit pomocí prostředí PowerShell, takže chcete Centrum událostí hello tooselect hello oboru názvů s hello název, který začíná **insights**.
  * Následující kód SQL Hello je ukázka Stream Analytics dotazu, které můžete tooparse všechna data protokolu hello v tabulce PowerBI tooa:

    ```sql
    SELECT
    records.ArrayValue.[Properties you want tootrack]
    INTO
    [OutputSourceName – hello PowerBI source]
    FROM
    [InputSourceName] AS e
    CROSS APPLY GetArrayElements(e.records) AS records
    ```

* **Vytvoření vlastní telemetrii a protokolování platformy** – Pokud už máte uživatelské telemetrie platformy nebo jsou právě přemýšlíte o vytváření jeden hello vysoce škálovatelné publikování a odběru povaha Event Hubs vám umožní tooflexibly ingestování diagnostiky protokoly. [V tématu Dana Rosanova Průvodce toousing Event Hubs telemetrie platformy globálním měřítku zde](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="enable-streaming-of-diagnostic-logs"></a>Povolení diagnostických protokolů streamování
Můžete povolit vysílání datového proudu diagnostické protokoly prostřednictvím kódu programu, prostřednictvím portálu hello, nebo pomocí hello [rozhraní REST API Azure monitorování](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings). V obou případech vytvoříte nastavení diagnostiky ve kterém můžete zadat na obor názvů služby Event Hubs a hello protokolu kategorií a metriky, které chcete toosend v oboru názvů toohello. Centra událostí je vytvořen v oboru názvů hello pro každou kategorii protokolu, které povolíte. Diagnostika **kategorie protokolu** je typ protokolu, který může shromažďovat prostředku.

> [!WARNING]
> Povolení a vysílání datového proudu diagnostických protokolů z výpočetní prostředky (například virtuální počítače nebo Service Fabric) [vyžaduje jinou sadu kroků](../event-hubs/event-hubs-streaming-azure-diags-data.md).
> 
> 

Hello Service Bus nebo Event Hubs obor názvů nemá toobe v hello stejného předplatného jako prostředek hello emitování protokoly tak dlouho, dokud hello uživatel, který konfiguruje nastavení hello má příslušné předplatné tooboth přístupu RBAC.

## <a name="stream-diagnostic-logs-using-hello-portal"></a>Datový proud diagnostické protokoly portálu hello
1. Hello portálu, přejděte tooAzure monitorování a klikněte na **nastavení diagnostiky**

    ![Monitorování části monitorování Azure](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-blade.png)

2. Volitelně můžete filtrovat seznam hello skupinu prostředků nebo typ prostředku, a potom klikněte na hello prostředků, pro kterou chcete tooset nastavení diagnostiky.

3. Pokud neexistuje žádné nastavení na hello prostředku, který jste zvolili, jste výzvami toocreate nastavení. Klikněte na tlačítko "Zapnout diagnostiky."

   ![Přidat nastavení diagnostiky - žádná existující nastavení](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-none.png)

   Pokud máte stávající nastavení na hello prostředku, zobrazí se seznam nastavení, které jsou již nakonfigurována na tomto prostředku. Klikněte na tlačítko "Přidat nastavení diagnostiky."

   ![Přidat nastavení diagnostiky - stávající nastavení](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-multiple.png)

3. Zadejte název nastavení a zaškrtněte políčko hello pro **centra událostí tooan datového proudu**, pak vyberte obor názvů Event Hubs.
   
   ![Přidat nastavení diagnostiky - stávající nastavení](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-configure.png)
    
   Hello vybraný obor názvů bude kde je hello centra událostí vytvořit (Pokud je to poprvé diagnostické protokoly streamování) nebo prostřednictvím datového proudu příliš (Pokud již existují prostředky, které jsou streamování tento obor názvů protokolu kategorie toothis), a definuje zásady hello hello oprávnění, která má streamování mechanismus hello. V současné době streamování tooan centra událostí vyžadují oprávnění spravovat, odeslání a naslouchání. Můžete vytvářet nebo upravovat zásady přístupu obor názvů sdílených Event Hubs portálu hello kartě hello konfigurace oboru názvů. tooupdate jednu z těchto nastavení diagnostiky, hello klienta musí mít oprávnění ListKey hello na hello Event Hubs autorizační pravidlo.

4. Klikněte na **Uložit**.

Po chvíli se hello nové nastavení se zobrazí v seznamu nastavení pro tento prostředek a diagnostické protokoly jsou streamování toothat účet úložiště, také se vygeneruje nová data události.

### <a name="via-powershell-cmdlets"></a>Pomocí rutin prostředí PowerShell
tooenable vysílání datového proudu prostřednictvím hello [rutin prostředí Azure PowerShell](insights-powershell-samples.md), můžete použít hello `Set-AzureRmDiagnosticSetting` rutiny s těmito parametry:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource ID] -ServiceBusRuleId [your Service Bus rule ID] -Enabled $true
```

Hello ID pravidla Service Bus je řetězec s Tento formát: `{Service Bus resource ID}/authorizationrules/{key name}`, například `/subscriptions/{subscription ID}/resourceGroups/Default-ServiceBus-WestUS/providers/Microsoft.ServiceBus/namespaces/{Service Bus namespace}/authorizationrules/RootManageSharedAccessKey`.

### <a name="via-azure-cli"></a>Prostřednictvím rozhraní příkazového řádku Azure
tooenable vysílání datového proudu prostřednictvím hello [rozhraní příkazového řádku Azure](insights-cli-samples.md), můžete použít hello `insights diagnostic set` příkaz takto:

```azurecli
azure insights diagnostic set --resourceId <resourceID> --serviceBusRuleId <serviceBusRuleID> --enabled true
```

Použijte hello stejný formát pro Service Bus pravidlo ID, jak je popsáno pro hello rutiny prostředí PowerShell.

## <a name="how-do-i-consume-hello-log-data-from-event-hubs"></a>Způsob, jakým využívají data protokolu hello ze služby Event Hubs?
Zde je ukázka výstupní data ze služby Event Hubs:

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

| Název elementu | Popis |
| --- | --- |
| záznamy |Pole všechny protokolu události v této datové části. |
| time |Čas, kdy hello k události došlo. |
| category |Kategorie protokolu pro tuto událost. |
| resourceId |ID prostředku hello prostředku, který vytvořil tuto událost. |
| operationName |Název operace hello. |
| úroveň |Volitelné. Určuje úroveň hello protokolu událostí. |
| properties |Vlastnosti události hello. |

Můžete zobrazit seznam všech poskytovatelů prostředků podporující streamování centra tooEvent [zde](monitoring-overview-of-diagnostic-logs.md).

## <a name="stream-data-from-compute-resources"></a>Datový proud dat z výpočetní prostředky
Můžete také stream diagnostických protokolů z výpočetní prostředky pomocí agenta Windows Azure Diagnostics hello. [Najdete v článku](../event-hubs/event-hubs-streaming-azure-diags-data.md) jak tooset této nahoru.

## <a name="next-steps"></a>Další kroky
* [Další informace o diagnostických protokolů Azure.](monitoring-overview-of-diagnostic-logs.md)
* [Začínáme s Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

