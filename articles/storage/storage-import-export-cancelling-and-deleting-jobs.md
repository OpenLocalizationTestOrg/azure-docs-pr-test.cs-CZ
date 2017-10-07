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
ms.openlocfilehash: 5d2aba510dafd0ca9a10f5643f721e7059a6a8f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="canceling-and-deleting-azure-importexport-jobs"></a>Zrušení a odstranění úlohy Azure Import/Export

Můžete požádat, aby úlohu zrušit před hello `Packaging` stavu pomocí volání hello [vlastnosti úlohy aktualizace](/rest/api/storageimportexport/jobs#Jobs_Update) operace a nastavení hello `CancelRequested` element příliš`true`. na základě typu best effort se zruší úlohu Hello. Pokud jednotky v hello proces přenosu dat, může data stále toobe přenést i poté, co bylo vyžádáno zrušení.

 Zrušené úlohy přesune toohello `Completed` stavu a uchovávat po dobu 90 dnů od této chvíle budou odstraněny.

 toodelete úlohu, volání hello [odstranit úlohu](/rest/api/storageimportexport/jobs#Jobs_Delete) operace předtím, než má dodaný hello úlohy (*tj*, zatímco úloha hello je v hello `Creating` stav). Můžete také odstranit úlohu, pokud je v hello `Completed` stavu. Po odstranění úlohy, její informace a stav již nejsou přístupné přes hello REST API nebo hello portálu Azure.

## <a name="next-steps"></a>Další kroky

* [Pomocí REST API služby importu a exportu hello](storage-import-export-using-the-rest-api.md)
