---
title: "aaaEnable diagnostické protokolování pro události Batch - Azure | Microsoft Docs"
description: "Zaznamenávat a analyzovat události protokolů diagnostiky pro prostředkům účet Azure Batch, jako jsou fondy a úkoly."
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: e14e611d-12cd-4671-91dc-bc506dc853e5
ms.service: batch
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: big-compute
ms.date: 05/22/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9d03303a3e857e9303f40cc6de5c32b5a51d8f8a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="log-events-for-diagnostic-evaluation-and-monitoring-of-batch-solutions"></a>Události protokolu diagnostiky hodnocení a sledování řešení Batch

Stejně jako u řadou služeb Azure, hello služba Batch vysílá události protokolu pro některé prostředky během hello doba platnosti prostředku hello. Můžete povolit Azure Batch diagnostické protokoly událostí toorecord prostředkům, jako jsou fondy a úkoly a pak použít hello protokolů diagnostiky hodnocení a sledování. Vytvořit událostmi, jako je fondu, fond odstranit, spuštění úloh, úloha ukončena a jiné jsou součástí Batch diagnostické protokoly.

> [!NOTE]
> Tento článek popisuje protokolování událostí pro prostředky na účtu Batch, sami, není úloh a úloh výstupní data. Informace o ukládání hello výstupních dat úloh a úloh, najdete v tématu [zachovat Azure Batch výstup úlohy a úkolů](batch-task-output.md).
> 
> 

## <a name="prerequisites"></a>Požadavky
* [Účet Azure Batch](batch-account-create-portal.md)
* [účet služby Azure Storage](../storage/common/storage-create-storage-account.md#create-a-storage-account)
  
  diagnostické protokoly toopersist Batch, musíte vytvořit účet úložiště Azure, kde Azure ukládat protokoly hello. Zadejte účet úložiště při jste [povolit protokolování diagnostiky](#enable-diagnostic-logging) vašeho účtu Batch. Hello účet úložiště, které zadáte, když povolíte protokol kolekce není hello stejné jako propojené úložiště účet odkazované tooin hello [balíčky aplikací](batch-application-packages.md) a [úloh výstup trvalost](batch-task-output.md) články.
  
  > [!WARNING]
  > Jste **účtovat** hello data uložená v účtu úložiště Azure. To zahrnuje hello diagnostické protokoly popsané v tomto článku. Mějte na paměti při navrhování vaší [protokolu zásady uchovávání informací](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md).
  > 
  > 

## <a name="enable-diagnostic-logging"></a>Povolení protokolování diagnostiky
Ve výchozím nastavení vašeho účtu Batch není povoleno protokolování diagnostiky. Je potřeba explicitně povolit protokolování diagnostiky pro každý účet Batch, že chcete toomonitor:

[Jak tooenable shromažďování diagnostických protokolů](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs)

Doporučujeme, abyste si přečetli hello úplné [přehled o Azure diagnostické protokoly](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) toogain článku pochopení nejen jak tooenable protokolování, ale hello protokolu kategorií nepodporuje hello různé služby Azure. Například Azure Batch aktuálně podporuje jednu kategorii protokolu: **protokoly služby**.

## <a name="service-logs"></a>Protokoly služby
Protokoly služby Azure Batch obsahovat události vygenerované službou Azure Batch hello během doby života hello prostředku Batch jako fondu nebo úloh. Každá událost vysílaných Batch je uložen v hello zadaný účet úložiště ve formátu JSON. To je třeba textu hello ukázkové **fondu vytvoření události**:

```json
{
    "poolId": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "4",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicatedComputeNodes": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoscale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

Každé události textu se nachází v .json souboru v hello zadaný účet úložiště Azure. Pokud chcete tooaccess hello protokoly přímo, můžete tooreview hello [schéma diagnostických protokolů v účtu úložiště hello](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md#schema-of-diagnostic-logs-in-the-storage-account).

## <a name="service-log-events"></a>Služba Protokol událostí
Hello služba Batch aktuálně vysílá hello následující služby protokolu událostí. Tento seznam nemusí být kompletní, protože další události mohly být přidány od poslední aktualizace v tomto článku.

| **Služba Protokol událostí** |
| --- |
| [Vytvoření fondu][pool_create] |
| [Spuštění odstranění fondu][pool_delete_start] |
| [Odstranění fondu dokončení][pool_delete_complete] |
| [Počáteční velikosti fondu][pool_resize_start] |
| [Dokončení změny velikosti fondu][pool_resize_complete] |
| [Spuštění úlohy][task_start] |
| [Dokončení úkolů][task_complete] |
| [Selhání úlohy][task_fail] |

## <a name="next-steps"></a>Další kroky
Přidání toostoring protokolů diagnostiky událostem v účtu Azure Storage, můžete také stream tooan události protokolu služby Batch [centra událostí Azure](../event-hubs/event-hubs-what-is-event-hubs.md)a odešlete je příliš[Azure Log Analytics](../log-analytics/log-analytics-overview.md).

* [Datový proud diagnostických protokolů Azure tooEvent rozbočovače](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md)
  
  Stream diagnostických událostí toohello vysoce škálovatelné data příjem příchozích dat služby Batch, do centra událostí. Centra událostí můžete přijímat miliony událostí za sekundu, které můžete transformovat a ukládat pomocí libovolného zprostředkovatele analýzu v reálném čase.
* [Analýza Azure diagnostických protokolů pomocí analýzy protokolů](../log-analytics/log-analytics-azure-storage.md)
  
  Odeslání vašeho tooLog diagnostické protokoly analýzy, kde můžete analyzovat, portálu hello Operations Management Suite (OMS), nebo exportovat pro analýzu v Power BI nebo Excelu.

[pool_create]: https://msdn.microsoft.com/library/azure/mt743615.aspx
[pool_delete_start]: https://msdn.microsoft.com/library/azure/mt743610.aspx
[pool_delete_complete]: https://msdn.microsoft.com/library/azure/mt743618.aspx
[pool_resize_start]: https://msdn.microsoft.com/library/azure/mt743609.aspx
[pool_resize_complete]: https://msdn.microsoft.com/library/azure/mt743608.aspx
[task_start]: https://msdn.microsoft.com/library/azure/mt743616.aspx
[task_complete]: https://msdn.microsoft.com/library/azure/mt743612.aspx
[task_fail]: https://msdn.microsoft.com/library/azure/mt743607.aspx
