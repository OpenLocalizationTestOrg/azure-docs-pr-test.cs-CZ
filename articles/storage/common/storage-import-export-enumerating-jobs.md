---
title: "Seznam všech úloh Azure Import/Export | MicrosoftDocs"
description: "Naučte se seznam všech úloh služby Azure Import/Export v předplatném."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: f2e619be-1bbd-4a54-9472-9e2f70a83b64
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 1977bfc0e516088310f45ecdd960287eeed2c2d8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="enumerating-jobs-in-the-azure-importexport-service"></a><span data-ttu-id="108a5-103">Vytváření výčtu úloh ve službě Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="108a5-103">Enumerating jobs in the Azure Import/Export service</span></span>
<span data-ttu-id="108a5-104">Chcete-li vytvořit výčet všech úloh v předplatném, volejte [seznamu úloh](/rest/api/storageimportexport/jobs#Jobs_List) operaci.</span><span class="sxs-lookup"><span data-stu-id="108a5-104">To enumerate all jobs in a subscription, call the [List Jobs](/rest/api/storageimportexport/jobs#Jobs_List) operation.</span></span> <span data-ttu-id="108a5-105">`List Jobs`Vrátí seznam úloh a také následující atributy:</span><span class="sxs-lookup"><span data-stu-id="108a5-105">`List Jobs` returns a list of jobs as well as the following attributes:</span></span>

-   <span data-ttu-id="108a5-106">Typ úlohy (Import nebo Export)</span><span class="sxs-lookup"><span data-stu-id="108a5-106">The type of job (Import or Export)</span></span>

-   <span data-ttu-id="108a5-107">Aktuální stav úlohy</span><span class="sxs-lookup"><span data-stu-id="108a5-107">The current job state</span></span>

-   <span data-ttu-id="108a5-108">Úloha přidruženému účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="108a5-108">The job's associated storage account</span></span>

## <a name="next-steps"></a><span data-ttu-id="108a5-109">Další kroky</span><span class="sxs-lookup"><span data-stu-id="108a5-109">Next steps</span></span>

* [<span data-ttu-id="108a5-110">Pomocí REST API služby importu a exportu</span><span class="sxs-lookup"><span data-stu-id="108a5-110">Using the Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)
