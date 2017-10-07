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
# <a name="canceling-and-deleting-azure-importexport-jobs"></a><span data-ttu-id="9dbef-103">Zrušení a odstranění úlohy Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="9dbef-103">Canceling and deleting Azure Import/Export jobs</span></span>

<span data-ttu-id="9dbef-104">Můžete požádat, aby úlohu zrušit před hello `Packaging` stavu pomocí volání hello [vlastnosti úlohy aktualizace](/rest/api/storageimportexport/jobs#Jobs_Update) operace a nastavení hello `CancelRequested` element příliš`true`.</span><span class="sxs-lookup"><span data-stu-id="9dbef-104">You can request that a job be cancelled before it is in hello `Packaging` state by calling hello [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) operation and setting hello `CancelRequested` element too`true`.</span></span> <span data-ttu-id="9dbef-105">na základě typu best effort se zruší úlohu Hello.</span><span class="sxs-lookup"><span data-stu-id="9dbef-105">hello job will be cancelled on a best-effort basis.</span></span> <span data-ttu-id="9dbef-106">Pokud jednotky v hello proces přenosu dat, může data stále toobe přenést i poté, co bylo vyžádáno zrušení.</span><span class="sxs-lookup"><span data-stu-id="9dbef-106">If drives are in hello process of transferring data, data may continue toobe transferred even after cancellation has been requested.</span></span>

 <span data-ttu-id="9dbef-107">Zrušené úlohy přesune toohello `Completed` stavu a uchovávat po dobu 90 dnů od této chvíle budou odstraněny.</span><span class="sxs-lookup"><span data-stu-id="9dbef-107">A cancelled job will move toohello `Completed` state and be kept for 90 days, at which point it will be deleted.</span></span>

 <span data-ttu-id="9dbef-108">toodelete úlohu, volání hello [odstranit úlohu](/rest/api/storageimportexport/jobs#Jobs_Delete) operace předtím, než má dodaný hello úlohy (*tj*, zatímco úloha hello je v hello `Creating` stav).</span><span class="sxs-lookup"><span data-stu-id="9dbef-108">toodelete a job, call hello [Delete Job](/rest/api/storageimportexport/jobs#Jobs_Delete) operation before hello job has shipped (*i.e.*, while hello job is in hello `Creating` state).</span></span> <span data-ttu-id="9dbef-109">Můžete také odstranit úlohu, pokud je v hello `Completed` stavu.</span><span class="sxs-lookup"><span data-stu-id="9dbef-109">You can also delete a job when it is in hello `Completed` state.</span></span> <span data-ttu-id="9dbef-110">Po odstranění úlohy, její informace a stav již nejsou přístupné přes hello REST API nebo hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9dbef-110">After a job has been deleted, its information and status are no longer accessible via hello REST API or hello Azure portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9dbef-111">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9dbef-111">Next steps</span></span>

* [<span data-ttu-id="9dbef-112">Pomocí REST API služby importu a exportu hello</span><span class="sxs-lookup"><span data-stu-id="9dbef-112">Using hello Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)
