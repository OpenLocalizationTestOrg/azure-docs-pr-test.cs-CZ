---
title: "Náhled využití disku pro úlohy exportu Azure Import/Export - v1 | Microsoft Docs"
description: "Informace o zobrazení náhledu seznam objektů BLOB, které jste vybrali pro úlohy exportu ve službě Azure Import/Export."
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
ms.openlocfilehash: 7bf74247090f91e17f81a9bc98ebfa78334c8c10
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="previewing-drive-usage-for-an-export-job"></a><span data-ttu-id="b0583-103">Náhled využití disku pro úlohu exportu</span><span class="sxs-lookup"><span data-stu-id="b0583-103">Previewing drive usage for an export job</span></span>
<span data-ttu-id="b0583-104">Než vytvoříte úlohy exportu, je třeba vybrat sadu objektů blob pro export.</span><span class="sxs-lookup"><span data-stu-id="b0583-104">Before you create an export job, you need to choose a set of blobs to be exported.</span></span> <span data-ttu-id="b0583-105">Službu Microsoft Azure Import/Export můžete použít seznam objektů blob cesty nebo objekt blob předpony k reprezentaci objektů BLOB, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="b0583-105">The Microsoft Azure Import/Export service allows you to use a list of blob paths or blob prefixes to represent the blobs you've selected.</span></span>  
  
<span data-ttu-id="b0583-106">Dále musíte určit, kolik jednotek, budete muset odeslat.</span><span class="sxs-lookup"><span data-stu-id="b0583-106">Next, you need to determine how many drives you need to send.</span></span> <span data-ttu-id="b0583-107">Tento nástroj Import/Export nabízí `PreviewExport` příkaz Náhled využití disku pro objekty BLOB, které jste vybrali, na základě velikosti jednotky se chystáte použít.</span><span class="sxs-lookup"><span data-stu-id="b0583-107">The Import/Export Tool provides the `PreviewExport` command to preview drive usage for the blobs you selected, based on the size of the drives you are going to use.</span></span>

## <a name="command-line-parameters"></a><span data-ttu-id="b0583-108">Parametry příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="b0583-108">Command-line parameters</span></span>

<span data-ttu-id="b0583-109">Při použití, můžete použít následující parametry `PreviewExport` příkaz nástroje importu a exportu.</span><span class="sxs-lookup"><span data-stu-id="b0583-109">You can use the following parameters when using the `PreviewExport` command of the Import/Export Tool.</span></span>

|<span data-ttu-id="b0583-110">Parametr příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="b0583-110">Command-line parameter</span></span>|<span data-ttu-id="b0583-111">Popis</span><span class="sxs-lookup"><span data-stu-id="b0583-111">Description</span></span>|  
|--------------------------|-----------------|  
|<span data-ttu-id="b0583-112">**/logdir:**< LogDirectory\></span><span class="sxs-lookup"><span data-stu-id="b0583-112">**/logdir:**<LogDirectory\></span></span>|<span data-ttu-id="b0583-113">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="b0583-113">Optional.</span></span> <span data-ttu-id="b0583-114">K adresáři protokolů.</span><span class="sxs-lookup"><span data-stu-id="b0583-114">The log directory.</span></span> <span data-ttu-id="b0583-115">Souborů podrobného protokolování se zapíšou do tohoto adresáře.</span><span class="sxs-lookup"><span data-stu-id="b0583-115">Verbose log files will be written to this directory.</span></span> <span data-ttu-id="b0583-116">Pokud není zadán žádný adresář protokolu, aktuální adresář se použije jako adresář protokolu.</span><span class="sxs-lookup"><span data-stu-id="b0583-116">If no log directory is specified, the current directory will be used as the log directory.</span></span>|  
|<span data-ttu-id="b0583-117">**/sn:**< StorageAccountName\></span><span class="sxs-lookup"><span data-stu-id="b0583-117">**/sn:**<StorageAccountName\></span></span>|<span data-ttu-id="b0583-118">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="b0583-118">Required.</span></span> <span data-ttu-id="b0583-119">Název účtu úložiště pro úlohy exportu.</span><span class="sxs-lookup"><span data-stu-id="b0583-119">The name of the storage account for the export job.</span></span>|  
|<span data-ttu-id="b0583-120">**/Sk:**< StorageAccountKey\></span><span class="sxs-lookup"><span data-stu-id="b0583-120">**/sk:**<StorageAccountKey\></span></span>|<span data-ttu-id="b0583-121">Vyžaduje, pokud není zadán sdíleného přístupového podpisu kontejneru.</span><span class="sxs-lookup"><span data-stu-id="b0583-121">Required if and only if a container SAS is not specified.</span></span> <span data-ttu-id="b0583-122">Klíč účtu pro účet úložiště pro úlohy exportu.</span><span class="sxs-lookup"><span data-stu-id="b0583-122">The account key for the storage account for the export job.</span></span>|  
|<span data-ttu-id="b0583-123">**/csas:**< ContainerSas\></span><span class="sxs-lookup"><span data-stu-id="b0583-123">**/csas:**<ContainerSas\></span></span>|<span data-ttu-id="b0583-124">Vyžaduje, pokud není zadaný klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="b0583-124">Required if and only if a storage account key is not specified.</span></span> <span data-ttu-id="b0583-125">Kontejner SAS pro výpis objekty BLOB ve úloha exportu.</span><span class="sxs-lookup"><span data-stu-id="b0583-125">The container SAS for listing the blobs to be exported in the export job.</span></span>|  
|<span data-ttu-id="b0583-126">**/ ExportBlobListFile:**< ExportBlobListFile\></span><span class="sxs-lookup"><span data-stu-id="b0583-126">**/ExportBlobListFile:**<ExportBlobListFile\></span></span>|<span data-ttu-id="b0583-127">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="b0583-127">Required.</span></span> <span data-ttu-id="b0583-128">Cesta k souboru XML soubor obsahující seznam objektů blob cesty nebo objektu blob předpony cestu pro export objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="b0583-128">Path to the XML file containing list of blob paths or blob path prefixes for the blobs to be exported.</span></span> <span data-ttu-id="b0583-129">Formát souboru, který je používán `BlobListBlobPath` element v [Put úlohy](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operace importu/exportu služby REST API.</span><span class="sxs-lookup"><span data-stu-id="b0583-129">The file format used in the `BlobListBlobPath` element in the [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operation of the Import/Export service REST API.</span></span>|  
|<span data-ttu-id="b0583-130">**/ DriveSize:**< DriveSize\></span><span class="sxs-lookup"><span data-stu-id="b0583-130">**/DriveSize:**<DriveSize\></span></span>|<span data-ttu-id="b0583-131">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="b0583-131">Required.</span></span> <span data-ttu-id="b0583-132">Velikost jednotky použijte pro úlohy exportu, *například*, 500 GB, 1,5 TB.</span><span class="sxs-lookup"><span data-stu-id="b0583-132">The size of drives to use for an export job, *e.g.*, 500GB, 1.5TB.</span></span>|  

## <a name="command-line-example"></a><span data-ttu-id="b0583-133">Příklad příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="b0583-133">Command-line example</span></span>

<span data-ttu-id="b0583-134">Následující příklad ukazuje `PreviewExport` příkaz:</span><span class="sxs-lookup"><span data-stu-id="b0583-134">The following example demonstrates the `PreviewExport` command:</span></span>  
  
```  
WAImportExport.exe PreviewExport /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /ExportBlobListFile:C:\WAImportExport\mybloblist.xml /DriveSize:500GB    
```  
  
<span data-ttu-id="b0583-135">Seznam souboru exportu objektu blob může obsahovat názvy objektů blob a objektů blob předpony, jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="b0583-135">The export blob list file may contain blob names and blob prefixes, as shown here:</span></span>  
  
```xml 
<?xml version="1.0" encoding="utf-8"?>  
<BlobList>  
<BlobPath>pictures/animals/koala.jpg</BlobPath>  
<BlobPathPrefix>/vhds/</BlobPathPrefix>  
<BlobPathPrefix>/movies/</BlobPathPrefix>  
</BlobList>  
```

<span data-ttu-id="b0583-136">Nástroj Azure Import/Export obsahuje seznam všech objektů blob pro export a vypočítá postup pack je do jednotky po zadanou velikost vezme v úvahu všechny nezbytné režijní náklady a pak odhadne počet jednotek, které jsou potřebné pro uložení objektů BLOB a informace o využití disku.</span><span class="sxs-lookup"><span data-stu-id="b0583-136">The Azure Import/Export Tool lists all blobs to be exported and calculates how to pack them into drives of the specified size, taking into account any necessary overhead, then estimates the number of drives needed to hold the blobs and drive usage information.</span></span>  
  
<span data-ttu-id="b0583-137">Tady je příklad výstupu s informační protokoly vynechání:</span><span class="sxs-lookup"><span data-stu-id="b0583-137">Here is an example of the output, with informational logs omitted:</span></span>  
  
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
  
## <a name="next-steps"></a><span data-ttu-id="b0583-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b0583-138">Next steps</span></span>

* [<span data-ttu-id="b0583-139">Referenční dokumentace Azure nástroj pro Import/Export</span><span class="sxs-lookup"><span data-stu-id="b0583-139">Azure Import/Export Tool reference</span></span>](storage-import-export-tool-how-to-v1.md)
