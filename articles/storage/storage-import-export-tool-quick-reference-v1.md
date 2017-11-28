---
title: "aaaQuick reference pro příkazy úlohy Azure Import/Export nástroj import - v1 | Microsoft Docs"
description: "Azure Import/Export nástroj informace o příkazech pro import často používané příkazy úlohy. Vztahuje se toov1 hello nástroj pro Import nebo Export."
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
ms.openlocfilehash: e36f065e5d23268758cf6b6db9428fe8a8e1056d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="quick-reference-for-frequently-used-commands-for-import-jobs"></a><span data-ttu-id="4aa1f-104">Stručná referenční příručka pro často používané příkazy pro úlohy importu</span><span class="sxs-lookup"><span data-stu-id="4aa1f-104">Quick reference for frequently used commands for import jobs</span></span>
<span data-ttu-id="4aa1f-105">Tato část poskytuje že rychlé odkazy na některé často používané příkazy.</span><span class="sxs-lookup"><span data-stu-id="4aa1f-105">This section provides a quick references for some frequently used commands.</span></span> <span data-ttu-id="4aa1f-106">Podrobné informace o použití, najdete v části [Příprava pevné disky pro úlohy importu](storage-import-export-tool-preparing-hard-drives-import-v1.md).</span><span class="sxs-lookup"><span data-stu-id="4aa1f-106">For detailed usage, see [Preparing Hard Drives for an Import Job](storage-import-export-tool-preparing-hard-drives-import-v1.md).</span></span>  

## <a name="prepare-hello-disks-when-data-already-copied-toohello-disks"></a><span data-ttu-id="4aa1f-107">Připravit disky hello při data již zkopírovat toohello disky</span><span class="sxs-lookup"><span data-stu-id="4aa1f-107">Prepare hello disks when data already copied toohello disks</span></span>
 <span data-ttu-id="4aa1f-108">Tady je ukázkový příkaz tooprepare disky při data již zkopírovat toohello pevný disk, který nebyl byla zatím nebyla šifrované pomocí nástroje BitLocker:</span><span class="sxs-lookup"><span data-stu-id="4aa1f-108">Here is a sample command tooprepare a disks when data already copied toohello hard drive that hasn't been yet been encrypted with BitLocker:</span></span>  
  
```  
  WAImportExport.exe PrepImport /j:9WM35C2V.jrn /id:session#1 /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /t:d /encrypt /srcdir:d:\movies\drama /dstdir:movies/drama/ /skipwrite
```    

## <a name="copy-a-single-directory-tooa-hard-drive"></a><span data-ttu-id="4aa1f-109">Zkopírujte na jednoho adresářového tooa pevný disk</span><span class="sxs-lookup"><span data-stu-id="4aa1f-109">Copy a single directory tooa hard drive</span></span>  
 <span data-ttu-id="4aa1f-110">Tady je ukázkový příkaz toocopy jednoho zdroje tooa directory pevný se disk, který nebyl byla zatím nebyla šifrované pomocí nástroje BitLocker:</span><span class="sxs-lookup"><span data-stu-id="4aa1f-110">Here is a sample command toocopy a single source directory tooa hard drive that hasn't been yet been encrypted with BitLocker:</span></span>  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
## <a name="copy-wwo-directories-tooa-hard-drive"></a><span data-ttu-id="4aa1f-111">Zkopírujte wwo adresáře tooa pevný disk</span><span class="sxs-lookup"><span data-stu-id="4aa1f-111">Copy wwo directories tooa hard drive</span></span>  
 <span data-ttu-id="4aa1f-112">toocopy dva zdrojového adresáře tooa jednotce, budete potřebovat dva příkazy.</span><span class="sxs-lookup"><span data-stu-id="4aa1f-112">toocopy two source directories tooa drive, you will need two commands.</span></span>  
  
 <span data-ttu-id="4aa1f-113">Určuje adresář protokolu hello, klíč účtu úložiště, písmeno jednotky cíl, Hello první příkaz a `format/encrypt` požadavky v přidání toohello společné parametry:</span><span class="sxs-lookup"><span data-stu-id="4aa1f-113">hello first command specifies hello log directory, storage account key, target drive letter and `format/encrypt` requirements, in addition toohello common parameters:</span></span>  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
 <span data-ttu-id="4aa1f-114">druhý příkaz Hello Určuje soubor hello deníku, nové ID relace a hello zdrojové a cílové umístění:</span><span class="sxs-lookup"><span data-stu-id="4aa1f-114">hello second command specifies hello journal file, a new session ID, and hello source and destination locations:</span></span>  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:music /srcdir:d:\Music /dstdir:entertainment/music/  
```  
  
## <a name="copy-a-large-file-tooa-hard-drive-in-a-second-copy-session"></a><span data-ttu-id="4aa1f-115">Kopírování velkého souboru tooa pevný disk v relaci druhé kopie</span><span class="sxs-lookup"><span data-stu-id="4aa1f-115">Copy a large file tooa hard drive in a second copy session</span></span>  
 <span data-ttu-id="4aa1f-116">Zde je ukázka příkaz, který zkopíruje jednotku tooa jeden velký soubor, který jste připravili v předchozí relaci kopie:</span><span class="sxs-lookup"><span data-stu-id="4aa1f-116">Here is a sample command that copies a single large file tooa drive that has been prepared in a previous copy session:</span></span>  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:dvd /srcfile:d:\dvd\favoritemovie.vhd /dstblob:dvd/favoritemovie.vhd  
```  
  
## <a name="next-steps"></a><span data-ttu-id="4aa1f-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4aa1f-117">Next steps</span></span>

* [<span data-ttu-id="4aa1f-118">Ukázkový pracovní postup tooprepare pevné disky pro úlohy importu</span><span class="sxs-lookup"><span data-stu-id="4aa1f-118">Sample workflow tooprepare hard drives for an import job</span></span>](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md)
