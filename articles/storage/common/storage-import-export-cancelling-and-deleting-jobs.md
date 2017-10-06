---
title: "aaaCancel a odstranit úlohu Azure Import/Export | Microsoft Docs"
description: "Zjistěte, jak toocancel a odstranění úlohy pro hello služby Microsoft Azure Import/Export."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: fd3d66f0-1dbb-4c75-9223-307d5abaeefc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 13456a8e7652850baacb53730cc7bb1520b0a4c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="canceling-and-deleting-azure-importexport-jobs"></a>Zrušení a odstranění úlohy Azure Import/Export

 toorequest zrušení úlohy dříve, než je v hello `Packaging` stav, volání hello [vlastnosti úlohy aktualizace](/rest/api/storageimportexport/jobs#Jobs_Update) operace a sadu hello `CancelRequested` element příliš`true`. Hello úlohy na základě typu best effort zrušeny. Pokud jednotky v hello proces přenosu dat, může data stále toobe přenést i poté, co bylo vyžádáno zrušení.

 Zrušené úlohy je přesunutý toohello `Completed` stavu a je uložen za 90 dnů od této chvíle se odstraní.

 toodelete úlohu, volání hello [odstranit úlohu](/rest/api/storageimportexport/jobs#Jobs_Delete) operace předtím, než má dodaný hello úlohy (to znamená, zatímco úloha hello je v hello `Creating` stav). Můžete také odstranit úlohu, pokud je v hello `Completed` stavu. Po odstranění úlohy její informace a stav již nejsou přístupné přes hello REST API nebo hello portálu Azure.

## <a name="next-steps"></a>Další kroky

* [Pomocí REST API služby importu a exportu hello](storage-import-export-using-the-rest-api.md)
