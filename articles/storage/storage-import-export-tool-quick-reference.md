---
title: "aaaQuick reference pro příkazy úlohy importu nástroj Azure Import/Export | Microsoft Docs"
description: "Azure Import/Export nástroj informace o příkazech pro import často používané příkazy úlohy."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: 0a615aed938e5e1b52d55a340aa6b48fa0744367
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="quick-reference-for-frequently-used-commands-for-import-jobs"></a><span data-ttu-id="ff38c-103">Stručná referenční příručka pro často používané příkazy pro úlohy importu</span><span class="sxs-lookup"><span data-stu-id="ff38c-103">Quick reference for frequently used commands for import jobs</span></span>

<span data-ttu-id="ff38c-104">Tento článek poskytuje že rychlý přehled o některé často používané příkazy.</span><span class="sxs-lookup"><span data-stu-id="ff38c-104">This article provides a quick reference for some frequently used commands.</span></span> <span data-ttu-id="ff38c-105">Podrobné informace o použití, najdete v části [Příprava pevné disky pro úlohy importu](storage-import-export-tool-preparing-hard-drives-import.md).</span><span class="sxs-lookup"><span data-stu-id="ff38c-105">For detailed usage, see [Preparing Hard Drives for an Import Job](storage-import-export-tool-preparing-hard-drives-import.md).</span></span>

## <a name="first-session"></a><span data-ttu-id="ff38c-106">První relaci</span><span class="sxs-lookup"><span data-stu-id="ff38c-106">First session</span></span>

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1 /sk:************* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

## <a name="second-session"></a><span data-ttu-id="ff38c-107">Druhý relace</span><span class="sxs-lookup"><span data-stu-id="ff38c-107">Second session</span></span>

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2 /DataSet:dataset-2.csv
```

## <a name="abort-latest-session"></a><span data-ttu-id="ff38c-108">Abort – nejnovější relace</span><span class="sxs-lookup"><span data-stu-id="ff38c-108">Abort latest session</span></span>

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2 /AbortSession
```

## <a name="resume-latest-interrupted-session"></a><span data-ttu-id="ff38c-109">Obnovte nejnovější dojde k přerušení relace</span><span class="sxs-lookup"><span data-stu-id="ff38c-109">Resume latest interrupted session</span></span>

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3 /ResumeSession
```

## <a name="add-drives-toolatest-session"></a><span data-ttu-id="ff38c-110">Přidání jednotky toolatest relace</span><span class="sxs-lookup"><span data-stu-id="ff38c-110">Add drives toolatest session</span></span>

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3 /AdditionalDriveSet:driveset-2.csv
```

## <a name="next-steps"></a><span data-ttu-id="ff38c-111">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ff38c-111">Next steps</span></span>

* [<span data-ttu-id="ff38c-112">Ukázkový pracovní postup tooprepare pevné disky pro úlohy importu</span><span class="sxs-lookup"><span data-stu-id="ff38c-112">Sample workflow tooprepare hard drives for an import job</span></span>](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow.md)
