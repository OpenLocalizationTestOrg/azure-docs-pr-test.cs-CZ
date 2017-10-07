---
title: aaaBacking si Azure Import/Export jednotky manifesty | Microsoft Docs
description: "Zjistěte, jak toohave jednotce manifesty pro službu Microsoft Azure Import/Export hello automaticky zálohovat."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 594abd80-b834-4077-a474-d8a0f4b7928a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: f48b97a2cce62714aace2b30a393305202c7ecd9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="backing-up-drive-manifests-for-azure-importexport-jobs"></a><span data-ttu-id="70db9-103">Zálohování jednotky manifesty pro úlohy Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="70db9-103">Backing up drive manifests for Azure Import/Export jobs</span></span>

<span data-ttu-id="70db9-104">Manifesty jednotky lze automaticky zálohovat tooblobs podle nastavení hello `BackupDriveManifest` vlastnost příliš`true` v hello [Put úlohy](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) nebo [vlastnosti úlohy aktualizace](/rest/api/storageimportexport/jobs#Jobs_Update) operace REST API.</span><span class="sxs-lookup"><span data-stu-id="70db9-104">Drive manifests can be automatically backed up tooblobs by setting hello `BackupDriveManifest` property too`true` in hello [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) or [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) REST API operations.</span></span> <span data-ttu-id="70db9-105">Ve výchozím nastavení nejsou manifesty jednotky hello zálohovat.</span><span class="sxs-lookup"><span data-stu-id="70db9-105">By default, hello drive manifests are not backed up.</span></span> <span data-ttu-id="70db9-106">Hello jednotky manifestu zálohy jsou uloženy jako objekty BLOB bloku v kontejneru v rámci účtu úložiště hello spojené s úlohou hello.</span><span class="sxs-lookup"><span data-stu-id="70db9-106">hello drive manifest backups are stored as block blobs in a container within hello storage account associated with hello job.</span></span> <span data-ttu-id="70db9-107">Ve výchozím nastavení, je název kontejneru hello `waimportexport`, ale můžete zadat jiný název v hello `DiagnosticsPath` vlastnost při volání metody hello `Put Job` nebo `Update Job Properties` operace.</span><span class="sxs-lookup"><span data-stu-id="70db9-107">By default, hello container name is `waimportexport`, but you can specify a different name in hello `DiagnosticsPath` property when calling hello `Put Job` or `Update Job Properties` operations.</span></span> <span data-ttu-id="70db9-108">Hello zálohování manifestu objekt blob se pojmenují podle hello následující formát: `waies/jobname_driveid_timestamp_manifest.xml`.</span><span class="sxs-lookup"><span data-stu-id="70db9-108">hello backup manifest blob are named in hello following format: `waies/jobname_driveid_timestamp_manifest.xml`.</span></span>

 <span data-ttu-id="70db9-109">Můžete načíst hello URI zálohování jednotky hello manifesty pro úlohu pomocí volání hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) operaci.</span><span class="sxs-lookup"><span data-stu-id="70db9-109">You can retrieve hello URI of hello backup drive manifests for a job by calling hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) operation.</span></span> <span data-ttu-id="70db9-110">Objekt blob Hello identifikátor URI je vrácený v hello `ManifestUri` vlastnosti pro každou jednotku.</span><span class="sxs-lookup"><span data-stu-id="70db9-110">hello blob URI is returned in hello `ManifestUri` property for each drive.</span></span>

## <a name="next-steps"></a><span data-ttu-id="70db9-111">Další kroky</span><span class="sxs-lookup"><span data-stu-id="70db9-111">Next steps</span></span>

* [<span data-ttu-id="70db9-112">Pomocí REST API služby importu a exportu hello</span><span class="sxs-lookup"><span data-stu-id="70db9-112">Using hello Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)
