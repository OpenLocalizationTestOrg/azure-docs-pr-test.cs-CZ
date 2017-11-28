---
title: "AAA \"události spuštění odstranění fondu Azure Batch | Microsoft Docs\""
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
ms.openlocfilehash: 79bb28bffc760a49cc0a95062f5086dc96c6a795
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="pool-delete-start-event"></a><span data-ttu-id="d2c63-103">Událost spuštění odstranění fondu</span><span class="sxs-lookup"><span data-stu-id="d2c63-103">Pool delete start event</span></span>

 <span data-ttu-id="d2c63-104">Tato událost je vygenerované při operaci odstranění fondu bylo zahájeno.</span><span class="sxs-lookup"><span data-stu-id="d2c63-104">This event is emitted when a pool delete operation has started.</span></span> <span data-ttu-id="d2c63-105">Vzhledem k tomu, že odstranění fondu hello je asynchronní událostí, můžete očekávat, že dokončení toobe fond odstranit událost complete vygenerované po operace odstranění hello.</span><span class="sxs-lookup"><span data-stu-id="d2c63-105">Since hello pool delete is an asynchronous event, you can expect a pool delete complete event toobe emitted once hello delete operation completes.</span></span>

 <span data-ttu-id="d2c63-106">Hello následující příklad ukazuje textu hello události spuštění odstranění fondu.</span><span class="sxs-lookup"><span data-stu-id="d2c63-106">hello following example shows hello body of a pool delete start event.</span></span>

```
{
    "id": "myPool1"
}
```

|<span data-ttu-id="d2c63-107">Element</span><span class="sxs-lookup"><span data-stu-id="d2c63-107">Element</span></span>|<span data-ttu-id="d2c63-108">Typ</span><span class="sxs-lookup"><span data-stu-id="d2c63-108">Type</span></span>|<span data-ttu-id="d2c63-109">Poznámky</span><span class="sxs-lookup"><span data-stu-id="d2c63-109">Notes</span></span>|
|-------------|----------|-----------|
|<span data-ttu-id="d2c63-110">id</span><span class="sxs-lookup"><span data-stu-id="d2c63-110">id</span></span>|<span data-ttu-id="d2c63-111">Řetězec</span><span class="sxs-lookup"><span data-stu-id="d2c63-111">String</span></span>|<span data-ttu-id="d2c63-112">Hello id fondu hello.</span><span class="sxs-lookup"><span data-stu-id="d2c63-112">hello id of hello pool.</span></span>|
