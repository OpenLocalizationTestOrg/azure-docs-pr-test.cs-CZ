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
# <a name="using-hello-azure-importexport-service-rest-api"></a>Pomocí REST API služby Azure Import/Export hello

Hello služby Microsoft Azure Import/Export zpřístupňuje rozhraní REST API tooenable programový ovládacího prvku úlohy importu a exportu. Můžete použít rozhraní API REST tooperform hello všechny hello importu a exportu operace, které můžete provádět s hello [portál Azure](https://portal.azure.com/). Kromě toho můžete použít hello REST API tooperform některé podrobné operace, např. dotazování hello procento dokončení úlohy, které nejsou momentálně k dispozici na portálu classic hello.

V tématu [pomocí hello Microsoft Azure Import/Export služby tooTransfer Data tooBlob úložiště](storage-import-export-service.md) přehled hello importu/exportu služby a kurz, který ukazuje, jak toouse hello portálu classic toocreate a spravovat import a export úloh.

## <a name="service-endpoints"></a>Koncové body služby

Hello služba Azure Import/Export je poskytovatel prostředků pro Azure Resource Manager a poskytuje sadu rozhraní API REST v hello následující koncový bod HTTPS pro správu úlohy importu a exportu:

```
https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.ImportExport/jobs/<job-name>
```

## <a name="versioning"></a>Správa verzí

Požadavky toohello importu/exportu služby musíte zadat hello `api-version` parametr a jeho hodnotu nastavte příliš`2016-11-01`.

## <a name="importexport-service-operations"></a>Import a Export operací služby

[Vytvoření úlohy importu](storage-import-export-creating-an-import-job.md)

[Vytvoření úlohy exportu](storage-import-export-creating-an-export-job.md)

[Načítání informací o stavu úlohy](storage-import-export-retrieving-state-info-for-a-job.md)

[Vytváření výčtu úloh](storage-import-export-enumerating-jobs.md)

[Rušení a odstraňování úloh](storage-import-export-cancelling-and-deleting-jobs.md)

[Manifesty zálohování jednotky](storage-import-export-backing-up-drive-manifests.md)

[Diagnostika a zotavení z chyb pro úlohy Import/export](storage-import-export-diagnostics-and-error-recovery.md)

## <a name="next-steps"></a>Další kroky

* [Import/Export úložiště REST](/rest/api/storageimportexport)
