---
title: "Analýza Azure Batch | Microsoft Docs"
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
ms.openlocfilehash: 7d634e1bb595a84b8af339e5bc5a483a7849e7f7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="batch-analytics"></a><span data-ttu-id="45189-103">Analýza batch</span><span class="sxs-lookup"><span data-stu-id="45189-103">Batch Analytics</span></span>
<span data-ttu-id="45189-104">Témata v Batch Analytics obsahovat informace o událostech a výstrahách, které jsou k dispozici pro prostředky služby Batch.</span><span class="sxs-lookup"><span data-stu-id="45189-104">The topics in Batch Analytics contain reference information for the events and alerts available for Batch service resources.</span></span>

<span data-ttu-id="45189-105">V tématu [protokolování diagnostiky Azure Batch](https://azure.microsoft.com/documentation/articles/batch-diagnostics/) Další informace o povolení a použití Batch diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="45189-105">See [Azure Batch diagnostic logging](https://azure.microsoft.com/documentation/articles/batch-diagnostics/) for more information on enabling and consuming Batch diagnostic logs.</span></span>

## <a name="diagnostic-logs"></a><span data-ttu-id="45189-106">Diagnostické protokoly</span><span class="sxs-lookup"><span data-stu-id="45189-106">Diagnostic logs</span></span>

<span data-ttu-id="45189-107">Službu Azure Batch vysílá následující události protokolů diagnostiky po dobu životnosti určitých prostředků Batch.</span><span class="sxs-lookup"><span data-stu-id="45189-107">The Azure Batch service emits the following diagnostic log events during the lifetime of certain Batch resources.</span></span>

<span data-ttu-id="45189-108">**Služba Protokol událostí**</span><span class="sxs-lookup"><span data-stu-id="45189-108">**Service Log events**</span></span>
* [<span data-ttu-id="45189-109">Vytvoření fondu</span><span class="sxs-lookup"><span data-stu-id="45189-109">Pool create</span></span>](batch-pool-create-event.md)
* [<span data-ttu-id="45189-110">Spuštění odstranění fondu</span><span class="sxs-lookup"><span data-stu-id="45189-110">Pool delete start</span></span>](batch-pool-delete-start-event.md)
* [<span data-ttu-id="45189-111">Odstranění fondu dokončení</span><span class="sxs-lookup"><span data-stu-id="45189-111">Pool delete complete</span></span>](batch-pool-delete-complete-event.md)
* [<span data-ttu-id="45189-112">Počáteční velikosti fondu</span><span class="sxs-lookup"><span data-stu-id="45189-112">Pool resize start</span></span>](batch-pool-resize-start-event.md)
* [<span data-ttu-id="45189-113">Dokončení změny velikosti fondu</span><span class="sxs-lookup"><span data-stu-id="45189-113">Pool resize complete</span></span>](batch-pool-resize-complete-event.md)
* [<span data-ttu-id="45189-114">Spuštění úlohy</span><span class="sxs-lookup"><span data-stu-id="45189-114">Task start</span></span>](batch-task-start-event.md)
* [<span data-ttu-id="45189-115">Dokončení úkolů</span><span class="sxs-lookup"><span data-stu-id="45189-115">Task complete</span></span>](batch-task-complete-event.md)
* [<span data-ttu-id="45189-116">Selhání úlohy</span><span class="sxs-lookup"><span data-stu-id="45189-116">Task fail</span></span>](batch-task-fail-event.md)