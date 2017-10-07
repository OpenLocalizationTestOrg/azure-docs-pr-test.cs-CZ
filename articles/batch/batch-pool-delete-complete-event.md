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
# <a name="pool-delete-complete-event"></a>Událost complete odstranění fondu

 Tato událost je vygenerované při operaci odstranění fondu byla dokončena.

 Hello následující příklad ukazuje textu hello události dokončení odstranění fondu.

```
{
    "id": "myPool1",
    "startTime": "2016-09-09T22:13:48.579Z",
    "endTime": "2016-09-09T22:14:08.836Z"
}
```

|Element|Typ|Poznámky|
|-------------|----------|-----------|
|id|Řetězec|Hello id fondu hello.|
|startTime|Data a času|čas Hello hello fond odstranit spuštění.|
|endTime|Data a času|Hello čas odstranění fondu hello dokončit.|

## <a name="remarks"></a>Poznámky
Další informace o stavu a kódy chyb pro operace změny velikosti fondu najdete v tématu [odstranění fondu z účtu](https://docs.microsoft.com/rest/api/batchservice/delete-a-pool-from-an-account).