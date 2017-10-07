---
title: "AAA \"fondu Azure Batch odstranit událost complete | Microsoft Docs\""
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
ms.openlocfilehash: 494c371e48ebfb1bf3d2973a7401829a939ba141
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="pool-delete-complete-event"></a><span data-ttu-id="a1928-103">Událost complete odstranění fondu</span><span class="sxs-lookup"><span data-stu-id="a1928-103">Pool delete complete event</span></span>

 <span data-ttu-id="a1928-104">Tato událost je vygenerované při operaci odstranění fondu byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="a1928-104">This event is emitted when a pool delete operation has completed.</span></span>

 <span data-ttu-id="a1928-105">Hello následující příklad ukazuje textu hello události dokončení odstranění fondu.</span><span class="sxs-lookup"><span data-stu-id="a1928-105">hello following example shows hello body of a pool delete complete event.</span></span>

```
{
    "id": "myPool1",
    "startTime": "2016-09-09T22:13:48.579Z",
    "endTime": "2016-09-09T22:14:08.836Z"
}
```

|<span data-ttu-id="a1928-106">Element</span><span class="sxs-lookup"><span data-stu-id="a1928-106">Element</span></span>|<span data-ttu-id="a1928-107">Typ</span><span class="sxs-lookup"><span data-stu-id="a1928-107">Type</span></span>|<span data-ttu-id="a1928-108">Poznámky</span><span class="sxs-lookup"><span data-stu-id="a1928-108">Notes</span></span>|
|-------------|----------|-----------|
|<span data-ttu-id="a1928-109">id</span><span class="sxs-lookup"><span data-stu-id="a1928-109">id</span></span>|<span data-ttu-id="a1928-110">Řetězec</span><span class="sxs-lookup"><span data-stu-id="a1928-110">String</span></span>|<span data-ttu-id="a1928-111">Hello id fondu hello.</span><span class="sxs-lookup"><span data-stu-id="a1928-111">hello id of hello pool.</span></span>|
|<span data-ttu-id="a1928-112">startTime</span><span class="sxs-lookup"><span data-stu-id="a1928-112">startTime</span></span>|<span data-ttu-id="a1928-113">Data a času</span><span class="sxs-lookup"><span data-stu-id="a1928-113">DateTime</span></span>|<span data-ttu-id="a1928-114">čas Hello hello fond odstranit spuštění.</span><span class="sxs-lookup"><span data-stu-id="a1928-114">hello time hello pool delete started.</span></span>|
|<span data-ttu-id="a1928-115">endTime</span><span class="sxs-lookup"><span data-stu-id="a1928-115">endTime</span></span>|<span data-ttu-id="a1928-116">Data a času</span><span class="sxs-lookup"><span data-stu-id="a1928-116">DateTime</span></span>|<span data-ttu-id="a1928-117">Hello čas odstranění fondu hello dokončit.</span><span class="sxs-lookup"><span data-stu-id="a1928-117">hello time hello pool delete completed.</span></span>|

## <a name="remarks"></a><span data-ttu-id="a1928-118">Poznámky</span><span class="sxs-lookup"><span data-stu-id="a1928-118">Remarks</span></span>
<span data-ttu-id="a1928-119">Další informace o stavu a kódy chyb pro operace změny velikosti fondu najdete v tématu [odstranění fondu z účtu](https://docs.microsoft.com/rest/api/batchservice/delete-a-pool-from-an-account).</span><span class="sxs-lookup"><span data-stu-id="a1928-119">For more information about states and error codes for pool resize operation, see [Delete a pool from an account](https://docs.microsoft.com/rest/api/batchservice/delete-a-pool-from-an-account).</span></span>