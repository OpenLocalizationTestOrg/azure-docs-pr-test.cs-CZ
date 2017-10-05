---
title: "Zálohování Azure Import/Export jednotky manifesty | Microsoft Docs"
description: "Zjistěte, jak má vaše manifesty jednotky pro službu Microsoft Azure Import/Export automaticky zálohovat."
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
ms.openlocfilehash: 33eb8e1eea8f8aa7b79ef3e54f2b1ed88dc794ae
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="backing-up-drive-manifests-for-azure-importexport-jobs"></a><span data-ttu-id="0212e-103">Zálohování jednotky manifesty pro úlohy Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="0212e-103">Backing up drive manifests for Azure Import/Export jobs</span></span>

<span data-ttu-id="0212e-104">Manifesty jednotky lze automaticky zálohovat na objekty BLOB nastavením `BackupDriveManifest` vlastnost `true` v [Put úlohy](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) nebo [vlastnosti úlohy aktualizace](/rest/api/storageimportexport/jobs#Jobs_Update) operace REST API.</span><span class="sxs-lookup"><span data-stu-id="0212e-104">Drive manifests can be automatically backed up to blobs by setting the `BackupDriveManifest` property to `true` in the [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) or [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) REST API operations.</span></span> <span data-ttu-id="0212e-105">Ve výchozím nastavení nejsou manifesty jednotky zálohovat.</span><span class="sxs-lookup"><span data-stu-id="0212e-105">By default, the drive manifests are not backed up.</span></span> <span data-ttu-id="0212e-106">Manifestu zálohy disku jsou uloženy jako objekty BLOB bloku v kontejneru v rámci účtu úložiště, které jsou přidružené k úloze.</span><span class="sxs-lookup"><span data-stu-id="0212e-106">The drive manifest backups are stored as block blobs in a container within the storage account associated with the job.</span></span> <span data-ttu-id="0212e-107">Ve výchozím nastavení, je název kontejneru `waimportexport`, ale můžete zadat jiný název v `DiagnosticsPath` vlastnost při volání metody `Put Job` nebo `Update Job Properties` operace.</span><span class="sxs-lookup"><span data-stu-id="0212e-107">By default, the container name is `waimportexport`, but you can specify a different name in the `DiagnosticsPath` property when calling the `Put Job` or `Update Job Properties` operations.</span></span> <span data-ttu-id="0212e-108">Zálohování manifestu blob jsou pojmenované v následujícím formátu: `waies/jobname_driveid_timestamp_manifest.xml`.</span><span class="sxs-lookup"><span data-stu-id="0212e-108">The backup manifest blob are named in the following format: `waies/jobname_driveid_timestamp_manifest.xml`.</span></span>

 <span data-ttu-id="0212e-109">Identifikátor URI manifesty zálohování jednotky pro úlohu můžete načíst pomocí volání [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) operaci.</span><span class="sxs-lookup"><span data-stu-id="0212e-109">You can retrieve the URI of the backup drive manifests for a job by calling the [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) operation.</span></span> <span data-ttu-id="0212e-110">Identifikátor URI je vrácený v objektu blob `ManifestUri` vlastnosti pro každou jednotku.</span><span class="sxs-lookup"><span data-stu-id="0212e-110">The blob URI is returned in the `ManifestUri` property for each drive.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0212e-111">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0212e-111">Next steps</span></span>

* [<span data-ttu-id="0212e-112">Pomocí REST API služby importu a exportu</span><span class="sxs-lookup"><span data-stu-id="0212e-112">Using the Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)
