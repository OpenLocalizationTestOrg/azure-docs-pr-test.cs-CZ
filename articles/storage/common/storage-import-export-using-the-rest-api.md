---
title: "Pomocí REST API služby Azure Import/Export | Microsoft Docs"
description: "Zjistěte, kde můžete najít prostředky pro používání služby Azure Import/Export rozhraní REST API, včetně postupy a referenční materiál."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 233f80e9-2e7f-48e0-9639-5c7785e7d743
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: b780385ad0af34bcb15639683d1aa5d689b38b50
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="using-the-azure-importexport-service-rest-api"></a><span data-ttu-id="6771a-103">Použití rozhraní REST API služby Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="6771a-103">Using the Azure Import/Export service REST API</span></span>

<span data-ttu-id="6771a-104">Službu Microsoft Azure Import/Export zpřístupňuje rozhraní REST API umožňující programovací řízení úlohy importu a exportu.</span><span class="sxs-lookup"><span data-stu-id="6771a-104">The Microsoft Azure Import/Export service exposes a REST API to enable programmatic control of import/export jobs.</span></span> <span data-ttu-id="6771a-105">Rozhraní REST API můžete provádět všechny operace importu a exportu, které můžete provádět pomocí [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="6771a-105">You can use the REST API to perform all of the import/export operations that you can perform with the [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="6771a-106">Kromě toho můžete použít rozhraní API REST k provádění některých granulární operací, jako je například dotazování procento dokončení úlohy, která není aktuálně k dispozici na portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="6771a-106">Additionally, you can use the REST API to perform certain granular operations, such as querying the percentage completion of a job, which is not currently available in the Azure classic portal.</span></span>

<span data-ttu-id="6771a-107">V tématu [pomocí služby Microsoft Azure Import/Export přenos dat do úložiště objektů Blob](../storage-import-export-service.md) přehled službu Import/Export a kurz, který ukazuje, jak použít klasický portál pro vytváření a správu import a export úloh.</span><span class="sxs-lookup"><span data-stu-id="6771a-107">See [Using the Microsoft Azure Import/Export service to Transfer Data to Blob Storage](../storage-import-export-service.md) for an overview of the Import/Export service and a tutorial that demonstrates how to use the classic portal to create and manage import and export jobs.</span></span>

## <a name="service-endpoints"></a><span data-ttu-id="6771a-108">Koncové body služby</span><span class="sxs-lookup"><span data-stu-id="6771a-108">Service endpoints</span></span>

<span data-ttu-id="6771a-109">Služba Azure Import/Export je poskytovatel prostředků pro Azure Resource Manager a poskytuje sadu rozhraní API REST v následující koncový bod HTTPS pro správu úlohy importu a exportu:</span><span class="sxs-lookup"><span data-stu-id="6771a-109">The Azure Import/Export service is a resource provider for Azure Resource Manager and provides a set of REST APIs at the following HTTPS endpoint for managing import/export jobs:</span></span>

```
https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.ImportExport/jobs/<job-name>
```

## <a name="versioning"></a><span data-ttu-id="6771a-110">Správa verzí</span><span class="sxs-lookup"><span data-stu-id="6771a-110">Versioning</span></span>

<span data-ttu-id="6771a-111">Musíte zadat žádosti o službu Import/Export `api-version` parametr a jeho hodnotu nastavte `2016-11-01`.</span><span class="sxs-lookup"><span data-stu-id="6771a-111">Requests to the Import/Export service must specify the `api-version` parameter and set its value to `2016-11-01`.</span></span>

## <a name="importexport-service-operations"></a><span data-ttu-id="6771a-112">Import a Export operací služby</span><span class="sxs-lookup"><span data-stu-id="6771a-112">Import/Export service operations</span></span>

[<span data-ttu-id="6771a-113">Vytvoření úlohy importu</span><span class="sxs-lookup"><span data-stu-id="6771a-113">Creating an import job</span></span>](../storage-import-export-creating-an-import-job.md)

[<span data-ttu-id="6771a-114">Vytvoření úlohy exportu</span><span class="sxs-lookup"><span data-stu-id="6771a-114">Creating an export job</span></span>](../storage-import-export-creating-an-export-job.md)

[<span data-ttu-id="6771a-115">Načítání informací o stavu úlohy</span><span class="sxs-lookup"><span data-stu-id="6771a-115">Retrieving state information for a job</span></span>](storage-import-export-retrieving-state-info-for-a-job.md)

[<span data-ttu-id="6771a-116">Vytváření výčtu úloh</span><span class="sxs-lookup"><span data-stu-id="6771a-116">Enumerating jobs</span></span>](../storage-import-export-enumerating-jobs.md)

[<span data-ttu-id="6771a-117">Rušení a odstraňování úloh</span><span class="sxs-lookup"><span data-stu-id="6771a-117">Cancelling and deleting jobs</span></span>](storage-import-export-cancelling-and-deleting-jobs.md)

[<span data-ttu-id="6771a-118">Manifesty zálohování jednotky</span><span class="sxs-lookup"><span data-stu-id="6771a-118">Backing Up drive manifests</span></span>](../storage-import-export-backing-up-drive-manifests.md)

[<span data-ttu-id="6771a-119">Diagnostika a zotavení z chyb pro úlohy Import/export</span><span class="sxs-lookup"><span data-stu-id="6771a-119">Diagnostics and error recovery for Import/Export jobs</span></span>](../storage-import-export-diagnostics-and-error-recovery.md)

## <a name="next-steps"></a><span data-ttu-id="6771a-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6771a-120">Next steps</span></span>

* [<span data-ttu-id="6771a-121">Import/Export úložiště REST</span><span class="sxs-lookup"><span data-stu-id="6771a-121">Storage Import/Export REST</span></span>](/rest/api/storageimportexport)
