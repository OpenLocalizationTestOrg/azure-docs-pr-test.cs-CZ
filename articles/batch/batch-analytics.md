---
title: aaaAzure Batch Analytics | Microsoft Docs
description: "Referenční informace pro analýzu Azure Batch."
services: batch
author: tamram
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: tamram
ms.openlocfilehash: 462fbad1ac522b485ae18c1e8891b9d2cabd45e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="batch-analytics"></a>Analýza batch
témata Hello v Batch Analytics obsahují referenční informace pro hello událostech a výstrahách, které jsou k dispozici pro prostředky služby Batch.

V tématu [protokolování diagnostiky Azure Batch](https://azure.microsoft.com/documentation/articles/batch-diagnostics/) Další informace o povolení a použití Batch diagnostické protokoly.

## <a name="diagnostic-logs"></a>Diagnostické protokoly

Hello služby Azure Batch vysílá hello následující události protokolů diagnostiky během doby života hello prostředků určité dávky.

**Služba Protokol událostí**
* [Vytvoření fondu](batch-pool-create-event.md)
* [Spuštění odstranění fondu](batch-pool-delete-start-event.md)
* [Odstranění fondu dokončení](batch-pool-delete-complete-event.md)
* [Počáteční velikosti fondu](batch-pool-resize-start-event.md)
* [Dokončení změny velikosti fondu](batch-pool-resize-complete-event.md)
* [Spuštění úlohy](batch-task-start-event.md)
* [Dokončení úkolů](batch-task-complete-event.md)
* [Selhání úlohy](batch-task-fail-event.md)