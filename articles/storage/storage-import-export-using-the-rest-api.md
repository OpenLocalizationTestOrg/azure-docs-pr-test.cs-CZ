---
title: "aaaUsing hello REST API služby Azure Import/Export | Microsoft Docs"
description: "Zjistěte, kde toofind prostředky pro používání hello Azure Import/Export služby REST API, včetně obou jak tooand referenční materiál."
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
ms.openlocfilehash: a01c170b1bc9c2b2ce9086d39de78a39fafb2c8b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-importexport-service-rest-api"></a><span data-ttu-id="34465-103">Pomocí REST API služby Azure Import/Export hello</span><span class="sxs-lookup"><span data-stu-id="34465-103">Using hello Azure Import/Export service REST API</span></span>

<span data-ttu-id="34465-104">Hello služby Microsoft Azure Import/Export zpřístupňuje rozhraní REST API tooenable programový ovládacího prvku úlohy importu a exportu.</span><span class="sxs-lookup"><span data-stu-id="34465-104">hello Microsoft Azure Import/Export service exposes a REST API tooenable programmatic control of import/export jobs.</span></span> <span data-ttu-id="34465-105">Můžete použít rozhraní API REST tooperform hello všechny hello importu a exportu operace, které můžete provádět s hello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="34465-105">You can use hello REST API tooperform all of hello import/export operations that you can perform with hello [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="34465-106">Kromě toho můžete použít hello REST API tooperform některé podrobné operace, např. dotazování hello procento dokončení úlohy, které nejsou momentálně k dispozici na portálu classic hello.</span><span class="sxs-lookup"><span data-stu-id="34465-106">Additionally, you can use hello REST API tooperform certain granular operations, such as querying hello percentage completion of a job, which are not currently available in hello classic portal.</span></span>

<span data-ttu-id="34465-107">V tématu [pomocí hello Microsoft Azure Import/Export služby tooTransfer Data tooBlob úložiště](storage-import-export-service.md) přehled hello importu/exportu služby a kurz, který ukazuje, jak toouse hello portálu classic toocreate a spravovat import a export úloh.</span><span class="sxs-lookup"><span data-stu-id="34465-107">See [Using hello Microsoft Azure Import/Export service tooTransfer Data tooBlob Storage](storage-import-export-service.md) for an overview of hello Import/Export service and a tutorial that demonstrates how toouse hello classic portal toocreate and manage import and export jobs.</span></span>

## <a name="service-endpoints"></a><span data-ttu-id="34465-108">Koncové body služby</span><span class="sxs-lookup"><span data-stu-id="34465-108">Service endpoints</span></span>

<span data-ttu-id="34465-109">Hello služba Azure Import/Export je poskytovatel prostředků pro Azure Resource Manager a poskytuje sadu rozhraní API REST v hello následující koncový bod HTTPS pro správu úlohy importu a exportu:</span><span class="sxs-lookup"><span data-stu-id="34465-109">hello Azure Import/Export service is a resource provider for Azure Resource Manager and provides a set of REST APIs at hello following HTTPS endpoint for managing import/export jobs:</span></span>

```
https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.ImportExport/jobs/<job-name>
```

## <a name="versioning"></a><span data-ttu-id="34465-110">Správa verzí</span><span class="sxs-lookup"><span data-stu-id="34465-110">Versioning</span></span>

<span data-ttu-id="34465-111">Požadavky toohello importu/exportu služby musíte zadat hello `api-version` parametr a jeho hodnotu nastavte příliš`2016-11-01`.</span><span class="sxs-lookup"><span data-stu-id="34465-111">Requests toohello Import/Export service must specify hello `api-version` parameter and set its value too`2016-11-01`.</span></span>

## <a name="importexport-service-operations"></a><span data-ttu-id="34465-112">Import a Export operací služby</span><span class="sxs-lookup"><span data-stu-id="34465-112">Import/Export service operations</span></span>

[<span data-ttu-id="34465-113">Vytvoření úlohy importu</span><span class="sxs-lookup"><span data-stu-id="34465-113">Creating an import job</span></span>](storage-import-export-creating-an-import-job.md)

[<span data-ttu-id="34465-114">Vytvoření úlohy exportu</span><span class="sxs-lookup"><span data-stu-id="34465-114">Creating an export job</span></span>](storage-import-export-creating-an-export-job.md)

[<span data-ttu-id="34465-115">Načítání informací o stavu úlohy</span><span class="sxs-lookup"><span data-stu-id="34465-115">Retrieving state information for a job</span></span>](storage-import-export-retrieving-state-info-for-a-job.md)

[<span data-ttu-id="34465-116">Vytváření výčtu úloh</span><span class="sxs-lookup"><span data-stu-id="34465-116">Enumerating jobs</span></span>](storage-import-export-enumerating-jobs.md)

[<span data-ttu-id="34465-117">Rušení a odstraňování úloh</span><span class="sxs-lookup"><span data-stu-id="34465-117">Cancelling and deleting jobs</span></span>](storage-import-export-cancelling-and-deleting-jobs.md)

[<span data-ttu-id="34465-118">Manifesty zálohování jednotky</span><span class="sxs-lookup"><span data-stu-id="34465-118">Backing Up drive manifests</span></span>](storage-import-export-backing-up-drive-manifests.md)

[<span data-ttu-id="34465-119">Diagnostika a zotavení z chyb pro úlohy Import/export</span><span class="sxs-lookup"><span data-stu-id="34465-119">Diagnostics and error recovery for Import/Export jobs</span></span>](storage-import-export-diagnostics-and-error-recovery.md)

## <a name="next-steps"></a><span data-ttu-id="34465-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="34465-120">Next steps</span></span>

* [<span data-ttu-id="34465-121">Import/Export úložiště REST</span><span class="sxs-lookup"><span data-stu-id="34465-121">Storage Import/Export REST</span></span>](/rest/api/storageimportexport)
