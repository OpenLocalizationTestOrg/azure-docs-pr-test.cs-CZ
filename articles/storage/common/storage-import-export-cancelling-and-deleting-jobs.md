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
ms.openlocfilehash: 1e989c72fc03697bf6d2e515ff53003703665d1a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="canceling-and-deleting-azure-importexport-jobs"></a><span data-ttu-id="1d59a-103">Zrušení a odstranění úlohy Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="1d59a-103">Canceling and deleting Azure Import/Export jobs</span></span>

 <span data-ttu-id="1d59a-104">Požádat o zrušit úlohu předtím, než je v `Packaging` stavu, volejte [vlastnosti úlohy aktualizace](/rest/api/storageimportexport/jobs#Jobs_Update) operace a sadu `CancelRequested` element `true`.</span><span class="sxs-lookup"><span data-stu-id="1d59a-104">To request that a job be canceled before it is in the `Packaging` state, call the [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) operation and set the `CancelRequested` element to `true`.</span></span> <span data-ttu-id="1d59a-105">Úloha je zrušena na základě typu best effort.</span><span class="sxs-lookup"><span data-stu-id="1d59a-105">The job is canceled on a best-effort basis.</span></span> <span data-ttu-id="1d59a-106">Pokud jednotky probíhá přenos dat, dat pokračovat v přesunu, i když bylo vyžádáno zrušení.</span><span class="sxs-lookup"><span data-stu-id="1d59a-106">If drives are in the process of transferring data, data may continue to be transferred even after cancellation has been requested.</span></span>

 <span data-ttu-id="1d59a-107">Zrušené úlohy je přesunuta do `Completed` stavu a je uložen za 90 dnů od této chvíle se odstraní.</span><span class="sxs-lookup"><span data-stu-id="1d59a-107">A canceled job is moved to the `Completed` state and is kept for 90 days, at which point it is deleted.</span></span>

 <span data-ttu-id="1d59a-108">Chcete-li odstranit úlohu, volejte [odstranit úlohu](/rest/api/storageimportexport/jobs#Jobs_Delete) operace předtím, než má dodaný úlohy (to znamená, když úloha je v `Creating` stav).</span><span class="sxs-lookup"><span data-stu-id="1d59a-108">To delete a job, call the [Delete Job](/rest/api/storageimportexport/jobs#Jobs_Delete) operation before the job has shipped (that is, while the job is in the `Creating` state).</span></span> <span data-ttu-id="1d59a-109">Můžete také odstranit úlohu, když je `Completed` stavu.</span><span class="sxs-lookup"><span data-stu-id="1d59a-109">You can also delete a job when it is in the `Completed` state.</span></span> <span data-ttu-id="1d59a-110">Po odstranění úlohy její informace a stav již nejsou přístupné přes rozhraní REST API nebo portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1d59a-110">After a job is deleted, its information and status are no longer accessible via the REST API or the Azure portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1d59a-111">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1d59a-111">Next steps</span></span>

* [<span data-ttu-id="1d59a-112">Pomocí REST API služby importu a exportu</span><span class="sxs-lookup"><span data-stu-id="1d59a-112">Using the Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)
