---
title: "aaaCollect Azure služby protokoly a metriky pro analýzy protokolů | Microsoft Docs"
description: "Konfigurace diagnostiky na prostředky Azure toowrite protokoly a metriky tooLog Analytics."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 84105740-3697-4109-bc59-2452c1131bfe
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1cede9a94ec83c4e3a95853dc2ec355d8df06d6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="collect-azure-service-logs-and-metrics-for-use-in-log-analytics"></a>Shromažďovat protokoly služby Azure a metriky pro použití v analýzy protokolů

Existují čtyři různé způsoby shromažďování protokolů a metriky pro služby Azure:

1. Azure diagnostics přímé tooLog Analytics (*diagnostiky* v hello následující tabulka)
2. Azure diagnostics tooAzure úložiště tooLog Analytics (*úložiště* v hello následující tabulka)
3. Konektory pro služby Azure (*konektory* v hello následující tabulka)
4. Skriptů toocollect a pak následná data do analýzy protokolů (prázdné buňky v následující tabulce hello a pro služby, které nejsou uvedené)


| Služba                 | Typ prostředku                           | Logs        | Metriky     | Řešení |
| --- | --- | --- | --- | --- |
| Application Gateway    | Microsoft.Network/applicationGateways   | Diagnostika | Diagnostika | [Analýza brány Azure aplikace](log-analytics-azure-networking-analytics.md#azure-application-gateway-analytics-solution-in-log-analytics) |
| Application insights    |                                         | konektor   | konektor   | [Konektor služby Statistika aplikace](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/) (Preview) |
| Účty Automation     | Microsoft.Automation/AutomationAccounts | Diagnostika |             | [Další informace](../automation/automation-manage-send-joblogs-log-analytics.md)|
| Účty batch          | Microsoft.Batch/batchAccounts           | Diagnostika | Diagnostika | |
| Classic cloudové služby  |                                         | Úložiště     |             | [Další informace](log-analytics-azure-storage-iis-table.md) |
| Kognitivní služby      | Microsoft.CognitiveServices/accounts    |             | Diagnostika | |
| Data Lake analytics     | Microsoft.DataLakeAnalytics/accounts    | Diagnostika |             | |
| Úložiště data Lake store         | Microsoft.DataLakeStore/accounts        | Diagnostika |             | |
| Názvový prostor události rozbočovače     | Microsoft.EventHub/namespaces           | Diagnostika | Diagnostika | |
| Centra IoT                | Microsoft.Devices/IotHubs               |             | Diagnostika | |
| Key Vault               | Microsoft.KeyVault/vaults               | Diagnostika |             | [KeyVault Analytics](log-analytics-azure-key-vault.md) |
| Nástroje pro vyrovnávání zatížení          | Microsoft.Network/loadBalancers         | Diagnostika |             |  |
| Logic Apps              | Microsoft.Logic/workflows <br> Microsoft.Logic/integrationAccounts | Diagnostika | Diagnostika | |
| Network Security Groups (Skupiny zabezpečení sítě) | Microsoft.Network/networksecuritygroups | Diagnostika |             | [Skupina zabezpečení sítě Azure Analytics](log-analytics-azure-networking-analytics.md#azure-network-security-group-analytics-solution-in-log-analytics) |
| Trezory zotavení         | Microsoft.RecoveryServices/vaults       |             |             | [Azure Recovery Services Analytics (Preview)](https://github.com/krnese/AzureDeploy/blob/master/OMS/MSOMS/Solutions/recoveryservices/)|
| Služby hledání         | Microsoft.Search/searchServices         | Diagnostika | Diagnostika | |
| Obor názvů Service Bus   | Microsoft.ServiceBus/namespaces         | Diagnostika | Diagnostika | [Service Bus Analytics (Preview)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-servicebus-solution)|
| Service Fabric          |                                         | Úložiště     |             | [Služba Fabric Analytics (Preview)](log-analytics-service-fabric.md) |
| SQL (v12)               | Microsoft.Sql/servers/databases <br> Microsoft.Sql/servers/elasticPools |             | Diagnostika | [Analýza Azure SQL (Preview)](log-analytics-azure-sql.md) |
| Úložiště                 |                                         |             | Skript      | [Azure Storage Analytics (Preview)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-azure-storage-analytics-solution) |
| Virtuální počítače        | Microsoft.Compute/virtualMachines       | Linka   | Linka <br> Diagnostika  | |
| Sady škálování virtuálních počítačů | Microsoft.Compute/virtualMachines <br> Microsoft.Compute/virtualMachineScaleSets/virtualMachines |             | Diagnostika | |
| Webové serverové farmy        | Microsoft.Web/serverfarms               |             | Diagnostika | |
| Weby               | Microsoft.Web/sites <br> Microsoft.Web/sites/slots |             | Diagnostika | [Službě Azure Web Apps Analytics (Preview)](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureWebAppsAnalyticsOMS?tab=Overview) |


> [!NOTE]
> U monitorování na virtuálních počítačích Azure (Linux a Windows), doporučujeme nainstalovat hello [rozšíření virtuálního počítače Log Analytics](log-analytics-azure-vm-extension.md). Hello agent vám poskytne přehled shromážděných z virtuálních počítačů. Můžete také použít hello rozšíření pro sady škálování virtuálního počítače.
>
>

## <a name="azure-diagnostics-direct-toolog-analytics"></a>Azure diagnostics přímé tooLog Analytics
Mnoho prostředků Azure jsou možné toowrite diagnostické protokoly a metriky přímo tooLog analýzy a tato je hello upřednostňovaný způsob shromažďování dat hello k analýze. Pokud používáte Azure diagnostics, data se zapisují okamžitě tooLog analýzy a není bez nutnosti toofirst zápisu hello data toostorage.

Prostředky Azure, které podporují [Azure monitorování](../monitoring-and-diagnostics/monitoring-overview.md) může poslat jejich protokoly a metriky přímo tooLog Analytics.

* Podrobnosti hello k dispozici metrik hello najdete příliš[podporované metriky s Azure monitorování](../monitoring-and-diagnostics/monitoring-supported-metrics.md).
* Podrobnosti hello hello k dispozici protokoly najdete příliš[podporované služby a schématu pro diagnostické protokoly](../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md).

### <a name="enable-diagnostics-with-powershell"></a>Povolení diagnostiky pomocí PowerShellu
Hello listopadu 2016 (v2.3.0) nebo novější vydání [prostředí Azure PowerShell](/powershell/azure/overview).

Následující příklad ukazuje prostředí PowerShell jak Hello toouse [Set-AzureRmDiagnosticSetting](/powershell/module/azurerm.insights/set-azurermdiagnosticsetting) tooenable diagnostics na skupinu zabezpečení sítě. Hello stejný přístup se dá použít pro všechny podporované prostředky – nastavit `$resourceId` id prostředku toohello chcete tooenable diagnostiky pro prostředek hello.

```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$resourceId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO"

Set-AzureRmDiagnosticSetting -ResourceId $ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="enable-diagnostics-with-resource-manager-templates"></a>Povolte diagnostiku pomocí šablony Resource Manageru

tooenable diagnostics na prostředek, když je vytvořen a odeslali hello diagnostiky tooyour pracovní prostor analýzy protokolů, které můžete použít šablonu podobné toohello, jeden níže. V tomto příkladu je pro účet Automation, ale funguje pro všechny typy podporovaných prostředků.

```json
        {
            "type": "Microsoft.Automation/automationAccounts/providers/diagnosticSettings",
            "name": "[concat(parameters('omsAutomationAccountName'), '/', 'Microsoft.Insights/service')]",
            "apiVersion": "2015-07-01",
            "dependsOn": [
                "[concat('Microsoft.Automation/automationAccounts/', parameters('omsAutomationAccountName'))]",
                "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspaceName'))]"
            ],
            "properties": {
                "workspaceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('omsWorkspaceName'))]",
                "logs": [
                    {
                        "category": "JobLogs",
                        "enabled": true
                    },
                    {
                        "category": "JobStreams",
                        "enabled": true
                    }
                ]
            }
        }
```

[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="azure-diagnostics-toostorage-then-toolog-analytics"></a>Azure diagnostics toostorage pak tooLog Analytics

Pro shromažďování protokolů z v rámci některé prostředky, je možné toosend hello protokoly tooAzure úložiště a pak nakonfigurujte analýzy protokolů tooread hello protokoly z úložiště.

Analýzy protokolů můžete použít tento přístup toocollect Diagnostika z úložiště Azure pro hello následující prostředky a protokoly:

| Prostředek | Logs |
| --- | --- |
| Service Fabric |ETWEvent <br> Provozních událostí <br> Událost spolehlivé objektu Actor <br> Spolehlivé služby událostí |
| Virtuální počítače |Linux Syslog <br> Události systému Windows <br> Protokol IIS <br> Windows ETWEvent |
| Webové role <br> Role pracovního procesu |Linux Syslog <br> Události systému Windows <br> Protokol IIS <br> Windows ETWEvent |

> [!NOTE]
> Budou se účtovat normální Azure datové sazby za úložiště a transakce při odeslání diagnostiky tooa účet úložiště a když analýzy protokolů čte hello data z účtu úložiště.
>
>

V tématu [pomocí úložiště objektů blob pro službu IIS a tabulka úložiště pro události](log-analytics-azure-storage-iis-table.md) toolearn více informací o jak analýzy protokolů shromáždit tyto protokoly.

## <a name="connectors-for-azure-services"></a>Konektory pro služby Azure

Je konektor pro službu Application Insights, což umožňuje data shromažďovaná společností odeslané tooLog Analytics toobe Application Insights.

Další informace o hello [Application Insights konektor](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/).

## <a name="scripts-toocollect-and-post-data-toolog-analytics"></a>Skripty toocollect a post data tooLog Analytics

Pro služby Azure, které neposkytuje tooLog přímý způsob toosend protokoly a metriky Analytics můžete použít Azure Automation skriptu toocollect hello protokolu a metriky. skript může Hello pak odeslat hello dat tooLog Analytics pomocí hello [kolekcí dat rozhraní API](log-analytics-data-collector-api.md)

Galerie Hello šablony Azure má [příklady použití Azure Automation](https://azure.microsoft.com/en-us/resources/templates/?term=OMS) toocollect data ze služby a odesláním tooLog Analytics.

## <a name="next-steps"></a>Další kroky

* [Používání úložiště blob pro službu IIS a tabulka úložiště pro události](log-analytics-azure-storage-iis-table.md) tooread hello protokoly služby Azure, které zápis diagnostiky tootable úložiště nebo IIS protokoluje napsané tooblob úložiště.
* [Povolit řešení](log-analytics-add-solutions.md) tooprovide vhled do dat hello.
* [Použijte vyhledávací dotazy](log-analytics-log-searches.md) tooanalyze hello data.
