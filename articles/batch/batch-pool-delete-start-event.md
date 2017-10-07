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
# <a name="pool-delete-start-event"></a>Událost spuštění odstranění fondu

 Tato událost je vygenerované při operaci odstranění fondu bylo zahájeno. Vzhledem k tomu, že odstranění fondu hello je asynchronní událostí, můžete očekávat, že dokončení toobe fond odstranit událost complete vygenerované po operace odstranění hello.

 Hello následující příklad ukazuje textu hello události spuštění odstranění fondu.

```
{
    "id": "myPool1"
}
```

|Element|Typ|Poznámky|
|-------------|----------|-----------|
|id|Řetězec|Hello id fondu hello.|
