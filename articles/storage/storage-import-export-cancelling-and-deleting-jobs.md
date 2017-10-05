---
title: "Zrušit a odstranit úlohu Azure Import/Export | Microsoft Docs"
description: "Zjistěte, jak zrušit a odstraňovat úlohy pro službu Microsoft Azure Import/Export."
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
ms.openlocfilehash: e0a7ff391e5a03ed563912dea54c7cfe73111bcf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="canceling-and-deleting-azure-importexport-jobs"></a>Zrušení a odstranění úlohy Azure Import/Export

Můžete požádat, aby úlohu zrušit předtím, než je v `Packaging` stavu voláním [vlastnosti úlohy aktualizace](/rest/api/storageimportexport/jobs#Jobs_Update) operace a nastavení `CancelRequested` element `true`. Na základě typu best effort se zruší úlohu. Pokud jednotky probíhá přenos dat, dat pokračovat v přesunu, i když bylo vyžádáno zrušení.

 Zrušené úlohy bude přesouvat `Completed` stavu a uchovávat po dobu 90 dnů od této chvíle budou odstraněny.

 Chcete-li odstranit úlohu, volejte [odstranit úlohu](/rest/api/storageimportexport/jobs#Jobs_Delete) operace předtím, než má dodaný úlohy (*tj*, zatímco úloha je v `Creating` stav). Můžete také odstranit úlohu, když je `Completed` stavu. Po odstranění úlohy, její informace a stav již nejsou přístupné přes rozhraní REST API nebo portálu Azure.

## <a name="next-steps"></a>Další kroky

* [Pomocí REST API služby importu a exportu](storage-import-export-using-the-rest-api.md)
