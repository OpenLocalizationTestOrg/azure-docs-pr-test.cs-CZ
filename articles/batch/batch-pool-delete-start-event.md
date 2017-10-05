---
title: "Události fondu počáteční delete Azure Batch | Microsoft Docs"
description: "Referenční informace pro odstranění fondu Batch, spusťte událost."
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
ms.openlocfilehash: f8a5241dce422e5c826ab428da6d7bc93284a1cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="pool-delete-start-event"></a><span data-ttu-id="b0fcb-103">Událost spuštění odstranění fondu</span><span class="sxs-lookup"><span data-stu-id="b0fcb-103">Pool delete start event</span></span>

 <span data-ttu-id="b0fcb-104">Tato událost je vygenerované při operaci odstranění fondu bylo zahájeno.</span><span class="sxs-lookup"><span data-stu-id="b0fcb-104">This event is emitted when a pool delete operation has started.</span></span> <span data-ttu-id="b0fcb-105">Vzhledem k tomu, že odstranění fondu je asynchronní událostí, můžete očekávat, že událost complete odstranění fondu pro vypuštění po dokončení operace odstranění.</span><span class="sxs-lookup"><span data-stu-id="b0fcb-105">Since the pool delete is an asynchronous event, you can expect a pool delete complete event to be emitted once the delete operation completes.</span></span>

 <span data-ttu-id="b0fcb-106">Následující příklad ukazuje tělo události spuštění odstranění fondu.</span><span class="sxs-lookup"><span data-stu-id="b0fcb-106">The following example shows the body of a pool delete start event.</span></span>

```
{
    "id": "myPool1"
}
```

|<span data-ttu-id="b0fcb-107">Element</span><span class="sxs-lookup"><span data-stu-id="b0fcb-107">Element</span></span>|<span data-ttu-id="b0fcb-108">Typ</span><span class="sxs-lookup"><span data-stu-id="b0fcb-108">Type</span></span>|<span data-ttu-id="b0fcb-109">Poznámky</span><span class="sxs-lookup"><span data-stu-id="b0fcb-109">Notes</span></span>|
|-------------|----------|-----------|
|<span data-ttu-id="b0fcb-110">id</span><span class="sxs-lookup"><span data-stu-id="b0fcb-110">id</span></span>|<span data-ttu-id="b0fcb-111">Řetězec</span><span class="sxs-lookup"><span data-stu-id="b0fcb-111">String</span></span>|<span data-ttu-id="b0fcb-112">Id fondu.</span><span class="sxs-lookup"><span data-stu-id="b0fcb-112">The id of the pool.</span></span>|