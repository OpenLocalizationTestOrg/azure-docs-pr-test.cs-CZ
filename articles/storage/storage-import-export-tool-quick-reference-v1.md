---
title: "Stručná referenční příručka pro úlohy příkazy pro import nástroj Azure Import/Export - v1 | Microsoft Docs"
description: "Azure Import/Export nástroj informace o příkazech pro import často používané příkazy úlohy. Vztahuje se k v1 nástroj importu a exportu."
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
ms.openlocfilehash: 47f450ee87dac3db2ccf7659928d52a6330a5697
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="quick-reference-for-frequently-used-commands-for-import-jobs"></a><span data-ttu-id="95833-104">Stručná referenční příručka pro často používané příkazy pro úlohy importu</span><span class="sxs-lookup"><span data-stu-id="95833-104">Quick reference for frequently used commands for import jobs</span></span>
<span data-ttu-id="95833-105">Tato část poskytuje že rychlé odkazy na některé často používané příkazy.</span><span class="sxs-lookup"><span data-stu-id="95833-105">This section provides a quick references for some frequently used commands.</span></span> <span data-ttu-id="95833-106">Podrobné informace o použití, najdete v části [Příprava pevné disky pro úlohy importu](storage-import-export-tool-preparing-hard-drives-import-v1.md).</span><span class="sxs-lookup"><span data-stu-id="95833-106">For detailed usage, see [Preparing Hard Drives for an Import Job](storage-import-export-tool-preparing-hard-drives-import-v1.md).</span></span>  

## <a name="prepare-the-disks-when-data-already-copied-to-the-disks"></a><span data-ttu-id="95833-107">Příprava disky, když data již zkopírují na disky</span><span class="sxs-lookup"><span data-stu-id="95833-107">Prepare the disks when data already copied to the disks</span></span>
 <span data-ttu-id="95833-108">Zde je ukázka příkazu, který připraví disky, když data již zkopírována na pevný disk, který nebyl byla zatím nebyla šifrované pomocí nástroje BitLocker:</span><span class="sxs-lookup"><span data-stu-id="95833-108">Here is a sample command to prepare a disks when data already copied to the hard drive that hasn't been yet been encrypted with BitLocker:</span></span>  
  
```  
  WAImportExport.exe PrepImport /j:9WM35C2V.jrn /id:session#1 /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /t:d /encrypt /srcdir:d:\movies\drama /dstdir:movies/drama/ /skipwrite
```    

## <a name="copy-a-single-directory-to-a-hard-drive"></a><span data-ttu-id="95833-109">Zkopírovat jeden adresář na pevný disk</span><span class="sxs-lookup"><span data-stu-id="95833-109">Copy a single directory to a hard drive</span></span>  
 <span data-ttu-id="95833-110">Zde je ukázka příkazu zkopírovat jeden zdrojový adresář na pevný disk, který nebyl byla zatím nebyla šifrované pomocí nástroje BitLocker:</span><span class="sxs-lookup"><span data-stu-id="95833-110">Here is a sample command to copy a single source directory to a hard drive that hasn't been yet been encrypted with BitLocker:</span></span>  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
## <a name="copy-wwo-directories-to-a-hard-drive"></a><span data-ttu-id="95833-111">Kopírování adresářů wwo na pevný disk</span><span class="sxs-lookup"><span data-stu-id="95833-111">Copy wwo directories to a hard drive</span></span>  
 <span data-ttu-id="95833-112">Pokud chcete zkopírovat dva adresáře zdroje na jednotku, budete potřebovat dva příkazy.</span><span class="sxs-lookup"><span data-stu-id="95833-112">To copy two source directories to a drive, you will need two commands.</span></span>  
  
 <span data-ttu-id="95833-113">První příkaz určuje adresář protokolu, klíč účtu úložiště, písmeno jednotky cíl a `format/encrypt` požadavky, kromě běžné parametry:</span><span class="sxs-lookup"><span data-stu-id="95833-113">The first command specifies the log directory, storage account key, target drive letter and `format/encrypt` requirements, in addition to the common parameters:</span></span>  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
 <span data-ttu-id="95833-114">V druhém příkazu Určuje soubor deníku, nové ID relace a zdrojové a cílové umístění:</span><span class="sxs-lookup"><span data-stu-id="95833-114">The second command specifies the journal file, a new session ID, and the source and destination locations:</span></span>  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:music /srcdir:d:\Music /dstdir:entertainment/music/  
```  
  
## <a name="copy-a-large-file-to-a-hard-drive-in-a-second-copy-session"></a><span data-ttu-id="95833-115">Kopírování velkého souboru na pevný disk v relaci druhé kopie</span><span class="sxs-lookup"><span data-stu-id="95833-115">Copy a large file to a hard drive in a second copy session</span></span>  
 <span data-ttu-id="95833-116">Zde je ukázka příkaz, který zkopíruje jeden velký soubor na disk, který jste připravili v předchozí relaci kopie:</span><span class="sxs-lookup"><span data-stu-id="95833-116">Here is a sample command that copies a single large file to a drive that has been prepared in a previous copy session:</span></span>  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:dvd /srcfile:d:\dvd\favoritemovie.vhd /dstblob:dvd/favoritemovie.vhd  
```  
  
## <a name="next-steps"></a><span data-ttu-id="95833-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="95833-117">Next steps</span></span>

* [<span data-ttu-id="95833-118">Ukázkový pracovní postup pro přípravu pevných disků pro úlohu importu</span><span class="sxs-lookup"><span data-stu-id="95833-118">Sample workflow to prepare hard drives for an import job</span></span>](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md)
