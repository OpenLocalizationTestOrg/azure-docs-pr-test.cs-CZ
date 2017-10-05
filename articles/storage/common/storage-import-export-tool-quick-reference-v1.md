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
ms.openlocfilehash: 370cf6fae7ae106e8341f65086c8b8187d335746
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="quick-reference-for-frequently-used-commands-for-import-jobs"></a><span data-ttu-id="d01c3-104">Stručná referenční příručka pro často používané příkazy pro úlohy importu</span><span class="sxs-lookup"><span data-stu-id="d01c3-104">Quick reference for frequently used commands for import jobs</span></span>
<span data-ttu-id="d01c3-105">Tato část poskytuje že rychlý přehled o některé často používané příkazy.</span><span class="sxs-lookup"><span data-stu-id="d01c3-105">This section provides a quick reference for some frequently used commands.</span></span> <span data-ttu-id="d01c3-106">Podrobné informace o použití, najdete v části [Příprava pevné disky pro úlohy importu](../storage-import-export-tool-preparing-hard-drives-import-v1.md).</span><span class="sxs-lookup"><span data-stu-id="d01c3-106">For detailed usage, see [Preparing Hard Drives for an Import Job](../storage-import-export-tool-preparing-hard-drives-import-v1.md).</span></span>  

## <a name="prepare-a-hard-drive-when-data-has-already-been-copied-to-the-hard-drive"></a><span data-ttu-id="d01c3-107">Příprava na pevný disk, když data již byla zkopírována do pevného disku</span><span class="sxs-lookup"><span data-stu-id="d01c3-107">Prepare a hard drive when data has already been copied to the hard drive</span></span>
 <span data-ttu-id="d01c3-108">Následující příkaz připraví na pevný disk, pokud již byl zkopírován do ní data, ale nebyla ještě šifrované pomocí nástroje BitLocker:</span><span class="sxs-lookup"><span data-stu-id="d01c3-108">The following command prepares a hard drive when data has already been copied to it, but has not yet been encrypted with BitLocker:</span></span>  
  
```  
  WAImportExport.exe PrepImport /j:9WM35C2V.jrn /id:session#1 /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /t:d /encrypt /srcdir:d:\movies\drama /dstdir:movies/drama/ /skipwrite
```    

## <a name="copy-a-single-directory-to-a-hard-drive"></a><span data-ttu-id="d01c3-109">Zkopírovat jeden adresář na pevný disk</span><span class="sxs-lookup"><span data-stu-id="d01c3-109">Copy a single directory to a hard drive</span></span>  
 <span data-ttu-id="d01c3-110">Následující příkaz zkopíruje na pevný disk, který nebyl ještě pomocí Bitlockeru šifrovat jeden zdrojový adresář:</span><span class="sxs-lookup"><span data-stu-id="d01c3-110">The following command copies a single source directory to a hard drive that has not yet been encrypted with BitLocker:</span></span>  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
## <a name="copy-two-directories-to-a-hard-drive"></a><span data-ttu-id="d01c3-111">Zkopírujte dva adresáře na pevný disk</span><span class="sxs-lookup"><span data-stu-id="d01c3-111">Copy two directories to a hard drive</span></span>  
 <span data-ttu-id="d01c3-112">Při kopírování dva adresáře zdroje na jednotku, použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="d01c3-112">To copy two source directories to a drive, use the following commands:</span></span>  
  
 <span data-ttu-id="d01c3-113">První příkaz určuje adresář protokolu, klíč účtu úložiště, cílový písmeno jednotky, `format/encrypt` požadavky a společné parametry:</span><span class="sxs-lookup"><span data-stu-id="d01c3-113">The first command specifies the log directory, storage account key, target drive letter, `format/encrypt` requirements, and common parameters:</span></span>  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
 <span data-ttu-id="d01c3-114">V druhém příkazu Určuje soubor deníku, nové ID relace a zdrojové a cílové umístění:</span><span class="sxs-lookup"><span data-stu-id="d01c3-114">The second command specifies the journal file, a new session ID, and the source and destination locations:</span></span>  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:music /srcdir:d:\Music /dstdir:entertainment/music/  
```  
  
## <a name="copy-a-large-file-to-a-hard-drive-in-a-second-copy-session"></a><span data-ttu-id="d01c3-115">Kopírování velkého souboru na pevný disk v relaci druhé kopie</span><span class="sxs-lookup"><span data-stu-id="d01c3-115">Copy a large file to a hard drive in a second copy session</span></span>  
 <span data-ttu-id="d01c3-116">Následující příkaz zkopíruje jeden velký soubor na pevný disk, který byl připraven v předchozí relaci kopie:</span><span class="sxs-lookup"><span data-stu-id="d01c3-116">The following command copies a single large file to a hard drive that was prepared in a previous copy session:</span></span>  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:dvd /srcfile:d:\dvd\favoritemovie.vhd /dstblob:dvd/favoritemovie.vhd  
```  
  
## <a name="next-steps"></a><span data-ttu-id="d01c3-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d01c3-117">Next steps</span></span>

* [<span data-ttu-id="d01c3-118">Ukázkový pracovní postup pro přípravu pevných disků pro úlohu importu</span><span class="sxs-lookup"><span data-stu-id="d01c3-118">Sample workflow to prepare hard drives for an import job</span></span>](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md)
