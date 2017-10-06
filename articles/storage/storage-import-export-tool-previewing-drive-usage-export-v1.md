---
title: "využívání jednotek aaaPreviewing úlohy exportu Azure Import/Export - v1 | Microsoft Docs"
description: "Zjistěte, jak toopreview hello seznam objektů BLOB jste vybrali pro úlohy exportu ve službě Azure Import/Export hello."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 7707d744-7ec7-4de8-ac9b-93a18608dc9a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: 88495f921371458c0451da6878fd7cc9a45d20cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="previewing-drive-usage-for-an-export-job"></a><span data-ttu-id="dee7d-103">Náhled využití disku pro úlohu exportu</span><span class="sxs-lookup"><span data-stu-id="dee7d-103">Previewing drive usage for an export job</span></span>
<span data-ttu-id="dee7d-104">Než vytvoříte úlohy exportu, je třeba toochoose sadu objektů BLOB toobe exportovali.</span><span class="sxs-lookup"><span data-stu-id="dee7d-104">Before you create an export job, you need toochoose a set of blobs toobe exported.</span></span> <span data-ttu-id="dee7d-105">Hello služby Microsoft Azure Import/Export můžete toouse seznam objektů blob cesty nebo objekt blob předpony toorepresent hello BLOB, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="dee7d-105">hello Microsoft Azure Import/Export service allows you toouse a list of blob paths or blob prefixes toorepresent hello blobs you've selected.</span></span>  
  
<span data-ttu-id="dee7d-106">Dále musíte toodetermine, kolik jednotek potřebujete toosend.</span><span class="sxs-lookup"><span data-stu-id="dee7d-106">Next, you need toodetermine how many drives you need toosend.</span></span> <span data-ttu-id="dee7d-107">Hello nástroj pro Import nebo Export poskytuje hello `PreviewExport` příkaz toopreview využívání jednotek pro objekty BLOB hello jste vybrali, na základě velikosti hello jednotek hello budete toouse.</span><span class="sxs-lookup"><span data-stu-id="dee7d-107">hello Import/Export Tool provides hello `PreviewExport` command toopreview drive usage for hello blobs you selected, based on hello size of hello drives you are going toouse.</span></span>

## <a name="command-line-parameters"></a><span data-ttu-id="dee7d-108">Parametry příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="dee7d-108">Command-line parameters</span></span>

<span data-ttu-id="dee7d-109">Můžete použít následující parametry při použití hello hello `PreviewExport` příkazu z hello nástroj pro Import nebo Export.</span><span class="sxs-lookup"><span data-stu-id="dee7d-109">You can use hello following parameters when using hello `PreviewExport` command of hello Import/Export Tool.</span></span>

|<span data-ttu-id="dee7d-110">Parametr příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="dee7d-110">Command-line parameter</span></span>|<span data-ttu-id="dee7d-111">Popis</span><span class="sxs-lookup"><span data-stu-id="dee7d-111">Description</span></span>|  
|--------------------------|-----------------|  
|<span data-ttu-id="dee7d-112">**/logdir:**< LogDirectory\></span><span class="sxs-lookup"><span data-stu-id="dee7d-112">**/logdir:**<LogDirectory\></span></span>|<span data-ttu-id="dee7d-113">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="dee7d-113">Optional.</span></span> <span data-ttu-id="dee7d-114">Adresář protokolu Hello.</span><span class="sxs-lookup"><span data-stu-id="dee7d-114">hello log directory.</span></span> <span data-ttu-id="dee7d-115">Podrobné soubory protokolu se zapíše toothis adresáře.</span><span class="sxs-lookup"><span data-stu-id="dee7d-115">Verbose log files will be written toothis directory.</span></span> <span data-ttu-id="dee7d-116">Pokud není zadán žádný adresář protokolu, aktuální adresář hello se použije jako adresář protokolu hello.</span><span class="sxs-lookup"><span data-stu-id="dee7d-116">If no log directory is specified, hello current directory will be used as hello log directory.</span></span>|  
|<span data-ttu-id="dee7d-117">**/sn:**< StorageAccountName\></span><span class="sxs-lookup"><span data-stu-id="dee7d-117">**/sn:**<StorageAccountName\></span></span>|<span data-ttu-id="dee7d-118">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="dee7d-118">Required.</span></span> <span data-ttu-id="dee7d-119">Úloha exportu Hello název účtu úložiště hello hello.</span><span class="sxs-lookup"><span data-stu-id="dee7d-119">hello name of hello storage account for hello export job.</span></span>|  
|<span data-ttu-id="dee7d-120">**/Sk:**< StorageAccountKey\></span><span class="sxs-lookup"><span data-stu-id="dee7d-120">**/sk:**<StorageAccountKey\></span></span>|<span data-ttu-id="dee7d-121">Vyžaduje, pokud není zadán sdíleného přístupového podpisu kontejneru.</span><span class="sxs-lookup"><span data-stu-id="dee7d-121">Required if and only if a container SAS is not specified.</span></span> <span data-ttu-id="dee7d-122">Úloha exportu Hello klíč účtu pro účet úložiště hello hello.</span><span class="sxs-lookup"><span data-stu-id="dee7d-122">hello account key for hello storage account for hello export job.</span></span>|  
|<span data-ttu-id="dee7d-123">**/csas:**< ContainerSas\></span><span class="sxs-lookup"><span data-stu-id="dee7d-123">**/csas:**<ContainerSas\></span></span>|<span data-ttu-id="dee7d-124">Vyžaduje, pokud není zadaný klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="dee7d-124">Required if and only if a storage account key is not specified.</span></span> <span data-ttu-id="dee7d-125">Hello kontejneru SAS pro výpis hello objekty BLOB toobe exportují do úlohy exportu hello.</span><span class="sxs-lookup"><span data-stu-id="dee7d-125">hello container SAS for listing hello blobs toobe exported in hello export job.</span></span>|  
|<span data-ttu-id="dee7d-126">**/ ExportBlobListFile:**< ExportBlobListFile\></span><span class="sxs-lookup"><span data-stu-id="dee7d-126">**/ExportBlobListFile:**<ExportBlobListFile\></span></span>|<span data-ttu-id="dee7d-127">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="dee7d-127">Required.</span></span> <span data-ttu-id="dee7d-128">Cesta toohello XML soubor obsahující seznam objektů blob cesty nebo objektu blob předpony cestu pro toobe objekty BLOB hello exportovali.</span><span class="sxs-lookup"><span data-stu-id="dee7d-128">Path toohello XML file containing list of blob paths or blob path prefixes for hello blobs toobe exported.</span></span> <span data-ttu-id="dee7d-129">Formát souboru Hello používá v hello `BlobListBlobPath` element v hello [Put úlohy](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operaci hello REST API služby importu a exportu.</span><span class="sxs-lookup"><span data-stu-id="dee7d-129">hello file format used in hello `BlobListBlobPath` element in hello [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operation of hello Import/Export service REST API.</span></span>|  
|<span data-ttu-id="dee7d-130">**/ DriveSize:**< DriveSize\></span><span class="sxs-lookup"><span data-stu-id="dee7d-130">**/DriveSize:**<DriveSize\></span></span>|<span data-ttu-id="dee7d-131">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="dee7d-131">Required.</span></span> <span data-ttu-id="dee7d-132">Hello velikost jednotky toouse pro úlohy exportu, *například*, 500 GB, 1,5 TB.</span><span class="sxs-lookup"><span data-stu-id="dee7d-132">hello size of drives toouse for an export job, *e.g.*, 500GB, 1.5TB.</span></span>|  

## <a name="command-line-example"></a><span data-ttu-id="dee7d-133">Příklad příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="dee7d-133">Command-line example</span></span>

<span data-ttu-id="dee7d-134">Hello následující příklad ukazuje hello `PreviewExport` příkaz:</span><span class="sxs-lookup"><span data-stu-id="dee7d-134">hello following example demonstrates hello `PreviewExport` command:</span></span>  
  
```  
WAImportExport.exe PreviewExport /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /ExportBlobListFile:C:\WAImportExport\mybloblist.xml /DriveSize:500GB    
```  
  
<span data-ttu-id="dee7d-135">Hello export souboru seznamu objektů blob může obsahovat názvy objektů blob a objektů blob předpony, jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="dee7d-135">hello export blob list file may contain blob names and blob prefixes, as shown here:</span></span>  
  
```xml 
<?xml version="1.0" encoding="utf-8"?>  
<BlobList>  
<BlobPath>pictures/animals/koala.jpg</BlobPath>  
<BlobPathPrefix>/vhds/</BlobPathPrefix>  
<BlobPathPrefix>/movies/</BlobPathPrefix>  
</BlobList>  
```

<span data-ttu-id="dee7d-136">Hello Azure Import/Export nástroj obsahuje seznam všech objektů BLOB toobe exportovali a vypočítá jak toopack do jednotky hello určená velikost, vezme v úvahu žádné režijní náklady na potřebné, pak odhadne hello počet jednotek potřebné objekty BLOB hello toohold a využití disku informace.</span><span class="sxs-lookup"><span data-stu-id="dee7d-136">hello Azure Import/Export Tool lists all blobs toobe exported and calculates how toopack them into drives of hello specified size, taking into account any necessary overhead, then estimates hello number of drives needed toohold hello blobs and drive usage information.</span></span>  
  
<span data-ttu-id="dee7d-137">Tady je příklad výstupu hello informační protokoly vynechání:</span><span class="sxs-lookup"><span data-stu-id="dee7d-137">Here is an example of hello output, with informational logs omitted:</span></span>  
  
```  
Number of unique blob paths/prefixes:   3  
Number of duplicate blob paths/prefixes:        0  
Number of nonexistent blob paths/prefixes:      1  
  
Drive size:     500.00 GB  
Number of blobs that can be exported:   6  
Number of blobs that cannot be exported:        2  
Number of drives needed:        3  
        Drive #1:       blobs = 1, occupied space = 454.74 GB  
        Drive #2:       blobs = 3, occupied space = 441.37 GB  
        Drive #3:       blobs = 2, occupied space = 131.28 GB    
```  
  
## <a name="next-steps"></a><span data-ttu-id="dee7d-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dee7d-138">Next steps</span></span>

* [<span data-ttu-id="dee7d-139">Referenční dokumentace Azure nástroj pro Import/Export</span><span class="sxs-lookup"><span data-stu-id="dee7d-139">Azure Import/Export Tool reference</span></span>](storage-import-export-tool-how-to-v1.md)
