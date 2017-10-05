---
title: "Odstranit událost complete fondu Azure Batch | Microsoft Docs"
description: "Referenční dokumentace pro fondu Batch odstranit událost complete."
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
ms.openlocfilehash: 890f2ba7fda37060c56177868d6214d517d91831
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="pool-delete-complete-event"></a><span data-ttu-id="0ecf7-103">Událost complete odstranění fondu</span><span class="sxs-lookup"><span data-stu-id="0ecf7-103">Pool delete complete event</span></span>

 <span data-ttu-id="0ecf7-104">Tato událost je vygenerované při operaci odstranění fondu byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="0ecf7-104">This event is emitted when a pool delete operation has completed.</span></span>

 <span data-ttu-id="0ecf7-105">Následující příklad ukazuje text událost complete odstranění fondu.</span><span class="sxs-lookup"><span data-stu-id="0ecf7-105">The following example shows the body of a pool delete complete event.</span></span>

```
{
    "id": "myPool1",
    "startTime": "2016-09-09T22:13:48.579Z",
    "endTime": "2016-09-09T22:14:08.836Z"
}
```

|<span data-ttu-id="0ecf7-106">Element</span><span class="sxs-lookup"><span data-stu-id="0ecf7-106">Element</span></span>|<span data-ttu-id="0ecf7-107">Typ</span><span class="sxs-lookup"><span data-stu-id="0ecf7-107">Type</span></span>|<span data-ttu-id="0ecf7-108">Poznámky</span><span class="sxs-lookup"><span data-stu-id="0ecf7-108">Notes</span></span>|
|-------------|----------|-----------|
|<span data-ttu-id="0ecf7-109">id</span><span class="sxs-lookup"><span data-stu-id="0ecf7-109">id</span></span>|<span data-ttu-id="0ecf7-110">Řetězec</span><span class="sxs-lookup"><span data-stu-id="0ecf7-110">String</span></span>|<span data-ttu-id="0ecf7-111">Id fondu.</span><span class="sxs-lookup"><span data-stu-id="0ecf7-111">The id of the pool.</span></span>|
|<span data-ttu-id="0ecf7-112">startTime</span><span class="sxs-lookup"><span data-stu-id="0ecf7-112">startTime</span></span>|<span data-ttu-id="0ecf7-113">Data a času</span><span class="sxs-lookup"><span data-stu-id="0ecf7-113">DateTime</span></span>|<span data-ttu-id="0ecf7-114">Odstranění fondu čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="0ecf7-114">The time the pool delete started.</span></span>|
|<span data-ttu-id="0ecf7-115">čas ukončení</span><span class="sxs-lookup"><span data-stu-id="0ecf7-115">endTime</span></span>|<span data-ttu-id="0ecf7-116">Data a času</span><span class="sxs-lookup"><span data-stu-id="0ecf7-116">DateTime</span></span>|<span data-ttu-id="0ecf7-117">Čas odstranění fondu byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="0ecf7-117">The time the pool delete completed.</span></span>|

## <a name="remarks"></a><span data-ttu-id="0ecf7-118">Poznámky</span><span class="sxs-lookup"><span data-stu-id="0ecf7-118">Remarks</span></span>
<span data-ttu-id="0ecf7-119">Další informace o stavu a kódy chyb pro operace změny velikosti fondu najdete v tématu [odstranění fondu z účtu](https://docs.microsoft.com/rest/api/batchservice/delete-a-pool-from-an-account).</span><span class="sxs-lookup"><span data-stu-id="0ecf7-119">For more information about states and error codes for pool resize operation, see [Delete a pool from an account](https://docs.microsoft.com/rest/api/batchservice/delete-a-pool-from-an-account).</span></span>