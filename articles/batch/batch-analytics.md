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
# <a name="batch-analytics"></a><span data-ttu-id="e3a69-103">Analýza batch</span><span class="sxs-lookup"><span data-stu-id="e3a69-103">Batch Analytics</span></span>
<span data-ttu-id="e3a69-104">témata Hello v Batch Analytics obsahují referenční informace pro hello událostech a výstrahách, které jsou k dispozici pro prostředky služby Batch.</span><span class="sxs-lookup"><span data-stu-id="e3a69-104">hello topics in Batch Analytics contain reference information for hello events and alerts available for Batch service resources.</span></span>

<span data-ttu-id="e3a69-105">V tématu [protokolování diagnostiky Azure Batch](https://azure.microsoft.com/documentation/articles/batch-diagnostics/) Další informace o povolení a použití Batch diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="e3a69-105">See [Azure Batch diagnostic logging](https://azure.microsoft.com/documentation/articles/batch-diagnostics/) for more information on enabling and consuming Batch diagnostic logs.</span></span>

## <a name="diagnostic-logs"></a><span data-ttu-id="e3a69-106">Diagnostické protokoly</span><span class="sxs-lookup"><span data-stu-id="e3a69-106">Diagnostic logs</span></span>

<span data-ttu-id="e3a69-107">Hello služby Azure Batch vysílá hello následující události protokolů diagnostiky během doby života hello prostředků určité dávky.</span><span class="sxs-lookup"><span data-stu-id="e3a69-107">hello Azure Batch service emits hello following diagnostic log events during hello lifetime of certain Batch resources.</span></span>

<span data-ttu-id="e3a69-108">**Služba Protokol událostí**</span><span class="sxs-lookup"><span data-stu-id="e3a69-108">**Service Log events**</span></span>
* [<span data-ttu-id="e3a69-109">Vytvoření fondu</span><span class="sxs-lookup"><span data-stu-id="e3a69-109">Pool create</span></span>](batch-pool-create-event.md)
* [<span data-ttu-id="e3a69-110">Spuštění odstranění fondu</span><span class="sxs-lookup"><span data-stu-id="e3a69-110">Pool delete start</span></span>](batch-pool-delete-start-event.md)
* [<span data-ttu-id="e3a69-111">Odstranění fondu dokončení</span><span class="sxs-lookup"><span data-stu-id="e3a69-111">Pool delete complete</span></span>](batch-pool-delete-complete-event.md)
* [<span data-ttu-id="e3a69-112">Počáteční velikosti fondu</span><span class="sxs-lookup"><span data-stu-id="e3a69-112">Pool resize start</span></span>](batch-pool-resize-start-event.md)
* [<span data-ttu-id="e3a69-113">Dokončení změny velikosti fondu</span><span class="sxs-lookup"><span data-stu-id="e3a69-113">Pool resize complete</span></span>](batch-pool-resize-complete-event.md)
* [<span data-ttu-id="e3a69-114">Spuštění úlohy</span><span class="sxs-lookup"><span data-stu-id="e3a69-114">Task start</span></span>](batch-task-start-event.md)
* [<span data-ttu-id="e3a69-115">Dokončení úkolů</span><span class="sxs-lookup"><span data-stu-id="e3a69-115">Task complete</span></span>](batch-task-complete-event.md)
* [<span data-ttu-id="e3a69-116">Selhání úlohy</span><span class="sxs-lookup"><span data-stu-id="e3a69-116">Task fail</span></span>](batch-task-fail-event.md)