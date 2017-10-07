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
# <a name="backing-up-drive-manifests-for-azure-importexport-jobs"></a>Zálohování jednotky manifesty pro úlohy Azure Import/Export

Manifesty jednotky lze automaticky zálohovat tooblobs podle nastavení hello `BackupDriveManifest` vlastnost příliš`true` v hello [Put úlohy](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) nebo [vlastnosti úlohy aktualizace](/rest/api/storageimportexport/jobs#Jobs_Update) operace REST API. Ve výchozím nastavení nejsou manifesty jednotky hello zálohovat. Hello jednotky manifestu zálohy jsou uloženy jako objekty BLOB bloku v kontejneru v rámci účtu úložiště hello spojené s úlohou hello. Ve výchozím nastavení, je název kontejneru hello `waimportexport`, ale můžete zadat jiný název v hello `DiagnosticsPath` vlastnost při volání metody hello `Put Job` nebo `Update Job Properties` operace. Hello zálohování manifestu objekt blob se pojmenují podle hello následující formát: `waies/jobname_driveid_timestamp_manifest.xml`.

 Můžete načíst hello URI zálohování jednotky hello manifesty pro úlohu pomocí volání hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) operaci. Objekt blob Hello identifikátor URI je vrácený v hello `ManifestUri` vlastnosti pro každou jednotku.

## <a name="next-steps"></a>Další kroky

* [Pomocí REST API služby importu a exportu hello](storage-import-export-using-the-rest-api.md)
