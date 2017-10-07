---
title: "aaaAzure diagnostické protokoly podporované služby a schémata | Microsoft Docs"
description: "Srozumitelná hello podporované služby a schématu události pro diagnostických protokolů Azure."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: fe8887df-b0e6-46f8-b2c0-11994d28e44f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: johnkem
ms.openlocfilehash: a3cbf5267e1bd0dc257f4fb4f42c323644046a6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="supported-services-schemas-and-categories-for-azure-diagnostic-logs"></a>Podporované služby, schémat a kategorie pro diagnostických protokolů Azure.

[Diagnostické protokoly prostředků Azure](monitoring-overview-of-diagnostic-logs.md) protokoly vysílaných vašich prostředků Azure, které popisují hello operace prostředku. Tyto protokoly jsou typu prostředku konkrétní. V tomto článku jsme popisují hello sadu podporovaných služeb a událostí schéma pro události vygenerované podle jednotlivých služeb. Tento článek také obsahuje úplný seznam dostupných protokolu kategorií podle typu prostředku.

## <a name="supported-services-and-schemas-for-resource-diagnostic-logs"></a>Podporované služby a schémat pro diagnostické protokoly prostředků
Hello schéma pro prostředek diagnostických protokolů se liší podle kategorie hello prostředků a protokolu.   

| Služba | Schéma & dokumentace |
| --- | --- |
| API Management | Schéma není k dispozici. |
| Brány Application Gateway |[Protokolování diagnostiky pro službu Application Gateway](../application-gateway/application-gateway-diagnostics.md) |
| Azure Automation |[Analýzy protokolů pro Azure Automation.](../automation/automation-manage-send-joblogs-log-analytics.md) |
| Azure Batch |[Protokolování diagnostiky Azure Batch](../batch/batch-diagnostics.md) |
| Statistika zákazníka | Schéma není k dispozici. |
| Content Delivery Network | Schéma není k dispozici. |
| CosmosDB | Schéma není k dispozici. |
| Data Lake Analytics |[Přístup k protokolům diagnostiky pro Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
| Data Lake Store |[Přístup k diagnostickým protokolům pro Azure Data Lake Store](../data-lake-store/data-lake-store-diagnostic-logs.md) |
| Event Hubs |[Diagnostické protokoly služby Azure Event Hubs](../event-hubs/event-hubs-diagnostic-logs.md) |
| Key Vault |[Protokolování v Azure Key Vault](../key-vault/key-vault-logging.md) |
| Load Balancer |[Log Analytics pro Azure Load Balancer](../load-balancer/load-balancer-monitor-log.md) |
| Logic Apps |[Vlastní schéma sledování B2B Logic Apps](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md) |
| Network Security Groups (Skupiny zabezpečení sítě) |[Analýza protokolu pro skupiny zabezpečení sítě (NSG)](../virtual-network/virtual-network-nsg-manage-log.md) |
| Recovery Services | Schéma není k dispozici.|
| Search |[Povolení a používání Analýza provozu vyhledávání](../search/search-traffic-analytics.md) |
| Správa serveru | Schéma není k dispozici. |
| Service Bus |[Diagnostické protokoly služby Azure Service Bus](../service-bus-messaging/service-bus-diagnostic-logs.md) |
| Stream Analytics |[Diagnostické protokoly úlohy](../stream-analytics/stream-analytics-job-diagnostic-logs.md) |

## <a name="supported-log-categories-per-resource-type"></a>Podporované kategorií protokolu na typ prostředku
|Typ prostředku|Kategorie|Zobrazovaný název kategorie|
|---|---|---|
|Microsoft.ApiManagement/service|GatewayLogs|Protokoly související tooApiManagement brány|
|Microsoft.Automation/automationAccounts|JobLogs|V protokolech úloh|
|Microsoft.Automation/automationAccounts|JobStreams|Datové proudy úlohy|
|Microsoft.Automation/automationAccounts|DscNodeStatus|Stav uzlu DSC|
|Microsoft.Batch/batchAccounts|ServiceLog|Protokoly služby|
|Microsoft.Cdn/profiles/endpoints|CoreAnalytics|Získá metriky hello hello koncového bodu, například šířky pásma, odchozí, atd.|
|Microsoft.CustomerInsights/hubs|AuditEvents|AuditEvents|
|Microsoft.DataLakeAnalytics/accounts|Auditování|Protokoly auditu|
|Microsoft.DataLakeAnalytics/accounts|Požadavky|Žádost o protokoly|
|Microsoft.DataLakeStore/accounts|Auditování|Protokoly auditu|
|Microsoft.DataLakeStore/accounts|Požadavky|Žádost o protokoly|
|Microsoft.DocumentDB/databaseAccounts|DataPlaneRequests|DataPlaneRequests|
|Microsoft.EventHub/namespaces|ArchiveLogs|Protokoly archivu|
|Microsoft.EventHub/namespaces|OperationalLogs|Operační protokoly|
|Microsoft.EventHub/namespaces|AutoScaleLogs|Automatické škálování protokoly|
|Microsoft.KeyVault/vaults|AuditEvent|Protokoly auditu|
|Microsoft.Logic/workflows|Modul runtime pracovního postupu|Diagnostických událostí modulu runtime pracovního postupu|
|Microsoft.Logic/integrationAccounts|IntegrationAccountTrackingEvents|Integrace účet sledovat události|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|Událost skupiny zabezpečení sítě|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|Čítač pravidel skupiny zabezpečení sítě|
|Microsoft.Network/loadBalancers|LoadBalancerAlertEvent|Události výstrah služby Vyrovnávání zatížení|
|Microsoft.Network/loadBalancers|LoadBalancerProbeHealthStatus|Stavu kontroly stavu služby Vyrovnávání zatížení|
|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|Aplikace v protokolu přístupu brány|
|Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|Aplikace v protokolu výkonu brány|
|Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|V protokolu aplikace brány Firewall|
|Microsoft.RecoveryServices/Vaults|AzureBackupReport|Zálohování Azure Data pro vytváření sestav|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryJobs|Úlohy Azure Site Recovery|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryEvents|Azure Site Recovery události|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicatedItems|Azure Site Recovery replikované položky|
|Microsoft.Search/searchServices|OperationLogs|Protokoly operací|
|Microsoft.ServiceBus/namespaces|OperationalLogs|Operační protokoly|
|Microsoft.StreamAnalytics/streamingjobs|Provádění|Provádění|
|Microsoft.StreamAnalytics/streamingjobs|Vytváření obsahu|Vytváření obsahu|

## <a name="next-steps"></a>Další kroky

* [Další informace o diagnostických protokolů](monitoring-overview-of-diagnostic-logs.md)
* [Stream diagnostické protokoly prostředků příliš**Event Hubs**](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [Změňte nastavení pro diagnostiku prostředků pomocí hello REST API služby Azure monitorování](https://msdn.microsoft.com/library/azure/dn931931.aspx)
* [Analýza protokolů z úložiště Azure s analýzy protokolů](../log-analytics/log-analytics-azure-storage.md)
