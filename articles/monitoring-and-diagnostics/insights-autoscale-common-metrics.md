---
title: "aaaAzure běžné metriky automatického škálování monitorování | Microsoft Docs"
description: "Další informace, které metriky běžně se používají pro automatické škálování cloudové služby, virtuální počítače a webové aplikace."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 189b2a13-01c8-4aca-afd5-90711903ca59
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/6/2016
ms.author: ancav
ms.openlocfilehash: 372a40d72d7a6c22c5ff854b1460ec8a3b7ed1d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-monitor-autoscaling-common-metrics"></a>Azure monitorování běžné metriky automatického škálování
Automatické škálování Azure monitorování vám umožní tooscale hello počet spuštěných instancích směrem nahoru nebo dolů na základě telemetrických dat (metriky). Tento dokument popisuje běžné metriky můžete chtít toouse. V hello portál Azure pro cloudové služby a serverové farmy můžete zvolit hello Metrika cíle hello tooscale prostředků pomocí. Můžete však žádné metrika z jiného prostředku tooscale podle.

Škálování Azure monitorování platí pouze příliš[sady škálování virtuálního počítače](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [cloudové služby](https://azure.microsoft.com/services/cloud-services/), a [služby App Service – webové aplikace](https://azure.microsoft.com/services/app-service/web/). Jinými službami Azure použít různé metody škálování.

## <a name="compute-metrics-for-resource-manager-based-vms"></a>Výpočetní metriky pro virtuální počítače na základě Resource Manager
Ve výchozím nastavení virtuální počítače využívající Resource Manager a sady škálování virtuálního počítače emitování základní metriky (na úrovni hostitele). Kromě toho při konfiguraci shromažďování dat diagnostiky pro virtuální počítač Azure a VMSS hello rozšíření diagnostiky Azure také vysílá čítače výkonu hostovaného operačního systému (často označované jako "Hostovaného operačního systému metrik").  Všechny tyto metriky můžete používat v pravidlech automatického škálování.

Můžete použít hello `Get MetricDefinitions` PoSH/API/CLI tooview hello metriky pro vaše VMSS prostředek k dispozici.

Pokud používáte škálovatelné sady virtuálních počítačů a nevidíte konkrétní metrika uvedené, pak je pravděpodobné, *zakázáno* ve vašem rozšíření diagnostiky.

Pokud konkrétní metriky není jen Vzorkovaná nebo přenášených v hello frekvence, které chcete, můžete aktualizovat konfiguraci diagnostiky hello.

Pokud se buď předchozím případě je hodnota true, zkontrolujte [tooenable použijte PowerShell Azure Diagnostics ve virtuálním počítači s Windows](../virtual-machines/windows/ps-extensions-diagnostics.md) o tooconfigure prostředí PowerShell a aktualizace vašeho tooenable rozšíření diagnostiky virtuálního počítače Azure hello metriku. Tento článek obsahuje také ukázkový soubor konfigurace diagnostiky.

### <a name="host-metrics-for-resource-manager-based-windows-and-linux-vms"></a>Metriky hostitele založené na správci prostředků systému Windows a virtuální počítače s Linuxem
Hello následující metriky na úrovni hostitele jsou vygenerované ve výchozím nastavení pro virtuální počítač Azure a VMSS v systému Windows a Linux instancí. Tyto metriky popisují svého virtuálního počítače Azure, ale jsou shromažďovány z hostitele virtuálního počítače Azure hello a nikoli prostřednictvím agent nainstalovaný na virtuálním počítači hosta hello. Tyto metriky můžete používat v pravidlech automatického škálování.

- [Metriky hostitele založené na správci prostředků systému Windows a virtuální počítače s Linuxem](monitoring-supported-metrics.md#microsoftcomputevirtualmachines)
- [Metriky hostitele založené na správci prostředků systému Windows a škálovatelné sady virtuálních počítačů Linux](monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets)

### <a name="guest-os-metrics-resource-manager-based-windows-vms"></a>Metriky hostovaného operačního systému založené na správci prostředků virtuálních počítačů Windows
Když vytvoříte virtuální počítač v Azure, je povolený diagnostiky pomocí rozšíření diagnostiky hello. rozšíření diagnostiky Hello vysílá sadu metriky, které jsou převzaty z uvnitř hello virtuálních počítačů. To znamená, že můžete použít automatické škálování z metriky, které nejsou vygenerované ve výchozím nastavení.

Seznam hello metriky můžete vygenerovat pomocí hello následující příkaz v prostředí PowerShell.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Můžete vytvořit výstrahu pro hello následující metriky:

| Název metriky | Jednotka |
| --- | --- |
| \Processor(_Total)\% Processor Time |Procento |
| \Processor(_Total)\% privilegovaného času |Procento |
| \Processor(_Total)\% uživatelského času |Procento |
| Frekvence \Processor \Processor informace (_celkem) |Počet |
| \System\Processes |Počet |
| Počet \Thread \Process (_celkem) |Počet |
| Počet \Handle \Process (_celkem) |Počet |
| \Memory\% využití potvrzených bajtů |Procento |
| \Memory\Available Bytes |Bajty |
| \Memory\Committed bajtů |Bajty |
| \Memory\Commit limit |Bajty |
| \Memory\Pool stránkovaného fondu |Bajty |
| \Memory\Pool nestránkovaného fondu |Bajty |
| \PhysicalDisk(_Total)\% čas na disku |Procento |
| \PhysicalDisk(_Total)\% čas čtení disku |Procento |
| \PhysicalDisk(_Total)\% doba zápisu na disku |Procento |
| Přenosy \Disk \PhysicalDisk (_celkem) za sekundu |CountPerSecond |
| Čtení \Disk \PhysicalDisk (_celkem) za sekundu |CountPerSecond |
| \PhysicalDisk (_celkem) \Disk zápisů za sekundu |CountPerSecond |
| \PhysicalDisk (_celkem) \Disk bajty/s |BytesPerSecond |
| Číst bajty/s \Disk \PhysicalDisk (_celkem) |BytesPerSecond |
| \PhysicalDisk (_celkem) \Disk zápisu bajty/s |BytesPerSecond |
| \Avg \PhysicalDisk (_celkem). Délka fronty disku |Počet |
| \Avg \PhysicalDisk (_celkem). Délka fronty disku pro čtení |Počet |
| \Avg \PhysicalDisk (_celkem). Délka fronty disku zápisu |Počet |
| \LogicalDisk(_Total)\% volného místa |Procento |
| Megabajtech \Free \LogicalDisk (_celkem) |Počet |

### <a name="guest-os-metrics-linux-vms"></a>Metriky hostovaného operačního systému Linux virtuální počítače
Když vytvoříte virtuální počítač v Azure, povolí se Diagnostika pomocí rozšíření diagnostiky ve výchozím nastavení.

Seznam hello metriky můžete vygenerovat pomocí hello následující příkaz v prostředí PowerShell.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

 Můžete vytvořit výstrahu pro hello následující metriky:

| Název metriky | Jednotka |
| --- | --- |
| \Memory\AvailableMemory |Bajty |
| \Memory\PercentAvailableMemory |Procento |
| \Memory\UsedMemory |Bajty |
| \Memory\PercentUsedMemory |Procento |
| \Memory\PercentUsedByCache |Procento |
| \Memory\PagesPerSec |CountPerSecond |
| \Memory\PagesReadPerSec |CountPerSecond |
| \Memory\PagesWrittenPerSec |CountPerSecond |
| \Memory\AvailableSwap |Bajty |
| \Memory\PercentAvailableSwap |Procento |
| \Memory\UsedSwap |Bajty |
| \Memory\PercentUsedSwap |Procento |
| \Processor\PercentIdleTime |Procento |
| \Processor\PercentUserTime |Procento |
| \Processor\PercentNiceTime |Procento |
| \Processor\PercentPrivilegedTime |Procento |
| \Processor\PercentInterruptTime |Procento |
| \Processor\PercentDPCTime |Procento |
| \Processor\PercentProcessorTime |Procento |
| \Processor\PercentIOWaitTime |Procento |
| \PhysicalDisk\BytesPerSecond |BytesPerSecond |
| \PhysicalDisk\ReadBytesPerSecond |BytesPerSecond |
| \PhysicalDisk\WriteBytesPerSecond |BytesPerSecond |
| \PhysicalDisk\TransfersPerSecond |CountPerSecond |
| \PhysicalDisk\ReadsPerSecond |CountPerSecond |
| \PhysicalDisk\WritesPerSecond |CountPerSecond |
| \PhysicalDisk\AverageReadTime |Sekundy |
| \PhysicalDisk\AverageWriteTime |Sekundy |
| \PhysicalDisk\AverageTransferTime |Sekundy |
| \PhysicalDisk\AverageDiskQueueLength |Počet |
| \NetworkInterface\BytesTransmitted |Bajty |
| \NetworkInterface\BytesReceived |Bajty |
| \NetworkInterface\PacketsTransmitted |Počet |
| \NetworkInterface\PacketsReceived |Počet |
| \NetworkInterface\BytesTotal |Bajty |
| \NetworkInterface\TotalRxErrors |Počet |
| \NetworkInterface\TotalTxErrors |Počet |
| \NetworkInterface\TotalCollisions |Počet |

## <a name="commonly-used-web-server-farm-metrics"></a>Běžně používané metriky webové (serverové farmě)
Můžete také provést automatické škálování podle běžné webové metriky server například hello délka fronty Http. Metriky název je **HttpQueueLength**.  Následující části Seznamy k dispozici server farmy (webové aplikace) metriky Hello.

### <a name="web-apps-metrics"></a>Webové aplikace metriky
Seznam hello webové aplikace metriky můžete vygenerovat pomocí hello následující příkaz v prostředí PowerShell.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Můžete výstrahy na nebo škálovat podle tyto metriky.

| Název metriky | Jednotka |
| --- | --- |
| CpuPercentage |Procento |
| MemoryPercentage |Procento |
| DiskQueueLength |Počet |
| HttpQueueLength |Počet |
| BytesReceived |Bajty |
| BytesSent |Bajty |

## <a name="commonly-used-storage-metrics"></a>Běžně používané metriky úložiště
Je možné škálovat podle délka fronty úložiště, což je hello počet zpráv ve frontě hello úložiště. Délka fronty úložiště je speciální metriky a prahová hodnota hello je hello počet zpráv na jednu instanci. Například pokud existují dvě instance a pokud je prahová hodnota hello nastavená too100, škálování nastane, když hello celkový počet zpráv ve frontě hello je 200. Který může být 100 zprávy na jednu instanci, 120 a 80 nebo jakoukoli jinou kombinaci, přidá too200 nebo více.

Nakonfigurujte toto nastavení v hello portál Azure hello **nastavení** okno. Pro škálovatelné sady virtuálních počítačů, můžete aktualizovat nastavení automatického škálování hello v toouse šablony Resource Manageru hello *metricName* jako *ApproximateMessageCount* a předejte hello ID fronty úložiště hello jako  *metricResourceUri*.

Například by metricTrigger nastavení automatického škálování s hello Classic účet úložiště patří:

```
"metricName": "ApproximateMessageCount",
 "metricNamespace": "",
 "metricResourceUri": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RES_GROUP_NAME/providers/Microsoft.ClassicStorage/storageAccounts/STORAGE_ACCOUNT_NAME/services/queue/queues/QUEUE_NAME"
 ```

Pro účet úložiště (ne klasický) by mělo zahrnovat hello metricTrigger:

```
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RES_GROUP_NAME/providers/Microsoft.Storage/storageAccounts/STORAGE_ACCOUNT_NAME/services/queue/queues/QUEUE_NAME"
```

## <a name="commonly-used-service-bus-metrics"></a>Běžně používané metriky služby Service Bus
Je možné škálovat podle délka fronty Service Bus, která je hello počet zpráv ve frontě Service Bus hello. Délka fronty Service Bus je speciální metriky a prahová hodnota hello je hello počet zpráv na jednu instanci. Například pokud existují dvě instance a pokud je prahová hodnota hello nastavená too100, škálování nastane, když hello celkový počet zpráv ve frontě hello je 200. Který může být 100 zprávy na jednu instanci, 120 a 80 nebo jakoukoli jinou kombinaci, přidá too200 nebo více.

Pro škálovatelné sady virtuálních počítačů, můžete aktualizovat nastavení automatického škálování hello v toouse šablony Resource Manageru hello *metricName* jako *ApproximateMessageCount* a předejte hello ID fronty úložiště hello jako  *metricResourceUri*.

```
"metricName": "MessageCount",
 "metricNamespace": "",
"metricResourceUri": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RES_GROUP_NAME/providers/Microsoft.ServiceBus/namespaces/SB_NAMESPACE/queues/QUEUE_NAME"
```

> [!NOTE]
> Služba Service Bus koncept skupiny prostředků hello neexistuje ale Azure Resource Manager vytvoří výchozí skupinu prostředků podle oblastí. Skupina prostředků Hello je obvykle ve formátu "Default - ServiceBus-[Oblast]" hello. Například: 'Výchozí-ServiceBus-EastUS', 'Výchozí-ServiceBus-WestUS', 'výchozí-ServiceBus-AustraliaEast"atd.
>
>
