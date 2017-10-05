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
# <a name="canceling-and-deleting-azure-importexport-jobs"></a><span data-ttu-id="ddb5f-103">Zrušení a odstranění úlohy Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="ddb5f-103">Canceling and deleting Azure Import/Export jobs</span></span>

<span data-ttu-id="ddb5f-104">Můžete požádat, aby úlohu zrušit předtím, než je v `Packaging` stavu voláním [vlastnosti úlohy aktualizace](/rest/api/storageimportexport/jobs#Jobs_Update) operace a nastavení `CancelRequested` element `true`.</span><span class="sxs-lookup"><span data-stu-id="ddb5f-104">You can request that a job be cancelled before it is in the `Packaging` state by calling the [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) operation and setting the `CancelRequested` element to `true`.</span></span> <span data-ttu-id="ddb5f-105">Na základě typu best effort se zruší úlohu.</span><span class="sxs-lookup"><span data-stu-id="ddb5f-105">The job will be cancelled on a best-effort basis.</span></span> <span data-ttu-id="ddb5f-106">Pokud jednotky probíhá přenos dat, dat pokračovat v přesunu, i když bylo vyžádáno zrušení.</span><span class="sxs-lookup"><span data-stu-id="ddb5f-106">If drives are in the process of transferring data, data may continue to be transferred even after cancellation has been requested.</span></span>

 <span data-ttu-id="ddb5f-107">Zrušené úlohy bude přesouvat `Completed` stavu a uchovávat po dobu 90 dnů od této chvíle budou odstraněny.</span><span class="sxs-lookup"><span data-stu-id="ddb5f-107">A cancelled job will move to the `Completed` state and be kept for 90 days, at which point it will be deleted.</span></span>

 <span data-ttu-id="ddb5f-108">Chcete-li odstranit úlohu, volejte [odstranit úlohu](/rest/api/storageimportexport/jobs#Jobs_Delete) operace předtím, než má dodaný úlohy (*tj*, zatímco úloha je v `Creating` stav).</span><span class="sxs-lookup"><span data-stu-id="ddb5f-108">To delete a job, call the [Delete Job](/rest/api/storageimportexport/jobs#Jobs_Delete) operation before the job has shipped (*i.e.*, while the job is in the `Creating` state).</span></span> <span data-ttu-id="ddb5f-109">Můžete také odstranit úlohu, když je `Completed` stavu.</span><span class="sxs-lookup"><span data-stu-id="ddb5f-109">You can also delete a job when it is in the `Completed` state.</span></span> <span data-ttu-id="ddb5f-110">Po odstranění úlohy, její informace a stav již nejsou přístupné přes rozhraní REST API nebo portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ddb5f-110">After a job has been deleted, its information and status are no longer accessible via the REST API or the Azure portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ddb5f-111">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ddb5f-111">Next steps</span></span>

* [<span data-ttu-id="ddb5f-112">Pomocí REST API služby importu a exportu</span><span class="sxs-lookup"><span data-stu-id="ddb5f-112">Using the Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)
