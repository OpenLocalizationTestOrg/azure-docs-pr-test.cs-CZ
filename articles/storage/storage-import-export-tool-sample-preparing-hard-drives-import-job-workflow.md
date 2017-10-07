---
title: "Úloha importu aaaSample pracovního postupu tooprep pevné disky pro Azure importu a exportu | Microsoft Docs"
description: "Návod pro dokončení procesu hello přípravy jednotky pro úlohy importu v hello služba Azure Import/Export najdete."
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
ms.date: 04/07/2017
ms.author: muralikk
ms.openlocfilehash: 560220b7dc9f87416f1fec1ff30fa5cd65812ce5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sample-workflow-tooprepare-hard-drives-for-an-import-job"></a><span data-ttu-id="8ca3f-103">Ukázkový pracovní postup tooprepare pevné disky pro úlohy importu</span><span class="sxs-lookup"><span data-stu-id="8ca3f-103">Sample workflow tooprepare hard drives for an import job</span></span>

<span data-ttu-id="8ca3f-104">Tento článek vás provede procesem dokončení hello přípravy jednotky pro úlohy importu.</span><span class="sxs-lookup"><span data-stu-id="8ca3f-104">This article walks you through hello complete process of preparing drives for an import job.</span></span>

## <a name="sample-data"></a><span data-ttu-id="8ca3f-105">Ukázková data</span><span class="sxs-lookup"><span data-stu-id="8ca3f-105">Sample data</span></span>

<span data-ttu-id="8ca3f-106">Tento příklad importuje následující data do účtu úložiště Azure s názvem hello `mystorageaccount`:</span><span class="sxs-lookup"><span data-stu-id="8ca3f-106">This example imports hello following data into an Azure storage account named `mystorageaccount`:</span></span>

|<span data-ttu-id="8ca3f-107">Umístění</span><span class="sxs-lookup"><span data-stu-id="8ca3f-107">Location</span></span>|<span data-ttu-id="8ca3f-108">Popis</span><span class="sxs-lookup"><span data-stu-id="8ca3f-108">Description</span></span>|<span data-ttu-id="8ca3f-109">Velikost dat</span><span class="sxs-lookup"><span data-stu-id="8ca3f-109">Data size</span></span>|
|--------------|-----------------|-----|
|<span data-ttu-id="8ca3f-110">H:\Video\\</span><span class="sxs-lookup"><span data-stu-id="8ca3f-110">H:\Video\\</span></span> |<span data-ttu-id="8ca3f-111">Kolekce videa</span><span class="sxs-lookup"><span data-stu-id="8ca3f-111">A collection of videos</span></span>|<span data-ttu-id="8ca3f-112">12 TB</span><span class="sxs-lookup"><span data-stu-id="8ca3f-112">12 TB</span></span>|
|<span data-ttu-id="8ca3f-113">H:\Photo\\</span><span class="sxs-lookup"><span data-stu-id="8ca3f-113">H:\Photo\\</span></span> |<span data-ttu-id="8ca3f-114">Kolekce fotografií</span><span class="sxs-lookup"><span data-stu-id="8ca3f-114">A collection of photos</span></span>|<span data-ttu-id="8ca3f-115">30 GB</span><span class="sxs-lookup"><span data-stu-id="8ca3f-115">30 GB</span></span>|
|<span data-ttu-id="8ca3f-116">K:\Temp\FavoriteMovie.ISO</span><span class="sxs-lookup"><span data-stu-id="8ca3f-116">K:\Temp\FavoriteMovie.ISO</span></span>|<span data-ttu-id="8ca3f-117">Bitová kopie disku A Blu-ray™</span><span class="sxs-lookup"><span data-stu-id="8ca3f-117">A Blu-Ray™ disk image</span></span>|<span data-ttu-id="8ca3f-118">25 GB</span><span class="sxs-lookup"><span data-stu-id="8ca3f-118">25 GB</span></span>|
|<span data-ttu-id="8ca3f-119">\\\bigshare\john\music\\</span><span class="sxs-lookup"><span data-stu-id="8ca3f-119">\\\bigshare\john\music\\</span></span>|<span data-ttu-id="8ca3f-120">Kolekce hudebních souborů ve sdílené síťové složce</span><span class="sxs-lookup"><span data-stu-id="8ca3f-120">A collection of music files on a network share</span></span>|<span data-ttu-id="8ca3f-121">10 GB</span><span class="sxs-lookup"><span data-stu-id="8ca3f-121">10 GB</span></span>|

## <a name="storage-account-destinations"></a><span data-ttu-id="8ca3f-122">Cíle účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="8ca3f-122">Storage account destinations</span></span>

<span data-ttu-id="8ca3f-123">Úloha importu Hello se importu dat hello do hello následující cíle v účtu úložiště hello:</span><span class="sxs-lookup"><span data-stu-id="8ca3f-123">hello import job will import hello data into hello following destinations in hello storage account:</span></span>

|<span data-ttu-id="8ca3f-124">Zdroj</span><span class="sxs-lookup"><span data-stu-id="8ca3f-124">Source</span></span>|<span data-ttu-id="8ca3f-125">Cílový virtuální adresář nebo objekt blob</span><span class="sxs-lookup"><span data-stu-id="8ca3f-125">Destination virtual directory or blob</span></span>|
|------------|-------------------------------------------|
|<span data-ttu-id="8ca3f-126">H:\Video\\</span><span class="sxs-lookup"><span data-stu-id="8ca3f-126">H:\Video\\</span></span> |<span data-ttu-id="8ca3f-127">video nebo</span><span class="sxs-lookup"><span data-stu-id="8ca3f-127">video/</span></span>|
|<span data-ttu-id="8ca3f-128">H:\Photo\\</span><span class="sxs-lookup"><span data-stu-id="8ca3f-128">H:\Photo\\</span></span> |<span data-ttu-id="8ca3f-129">fotografie nebo</span><span class="sxs-lookup"><span data-stu-id="8ca3f-129">photo/</span></span>|
|<span data-ttu-id="8ca3f-130">K:\Temp\FavoriteMovie.ISO</span><span class="sxs-lookup"><span data-stu-id="8ca3f-130">K:\Temp\FavoriteMovie.ISO</span></span>|<span data-ttu-id="8ca3f-131">favorite/FavoriteMovies.ISO</span><span class="sxs-lookup"><span data-stu-id="8ca3f-131">favorite/FavoriteMovies.ISO</span></span>|
|<span data-ttu-id="8ca3f-132">\\\bigshare\john\music\\</span><span class="sxs-lookup"><span data-stu-id="8ca3f-132">\\\bigshare\john\music\\</span></span> |<span data-ttu-id="8ca3f-133">Hudba</span><span class="sxs-lookup"><span data-stu-id="8ca3f-133">music</span></span>|

<span data-ttu-id="8ca3f-134">Pomocí této mapování hello souboru `H:\Video\Drama\GreatMovie.mov` bude objekt blob importované toohello `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.</span><span class="sxs-lookup"><span data-stu-id="8ca3f-134">With this mapping, hello file `H:\Video\Drama\GreatMovie.mov` will be imported toohello blob `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.</span></span>

## <a name="determine-hard-drive-requirements"></a><span data-ttu-id="8ca3f-135">Stanovení požadavků na pevném disku</span><span class="sxs-lookup"><span data-stu-id="8ca3f-135">Determine hard drive requirements</span></span>

<span data-ttu-id="8ca3f-136">Dále toodetermine kolik pevné disky jsou potřeba, výpočetní hello velikost dat hello:</span><span class="sxs-lookup"><span data-stu-id="8ca3f-136">Next, toodetermine how many hard drives are needed, compute hello size of hello data:</span></span>

`12TB + 30GB + 25GB + 10GB = 12TB + 65GB`

<span data-ttu-id="8ca3f-137">V tomto příkladu by mělo být dostatečné dva 8TB pevné disky.</span><span class="sxs-lookup"><span data-stu-id="8ca3f-137">For this example, two 8TB hard drives should be sufficient.</span></span> <span data-ttu-id="8ca3f-138">Ale protože hello zdrojový adresář `H:\Video` má 12TB dat a jeden pevný disk je kapacita je pouze 8TB, bude možné toospecify v hello následující způsobem hello **driveset.csv** souboru:</span><span class="sxs-lookup"><span data-stu-id="8ca3f-138">However, since hello source directory `H:\Video` has 12TB of data and your single hard drive's capacity is only 8TB, you will be able toospecify this in hello following way in hello **driveset.csv** file:</span></span>

```
DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
X,Format,SilentMode,Encrypt,
Y,Format,SilentMode,Encrypt,
```
<span data-ttu-id="8ca3f-139">Nástroj Hello provede distribuci dat mezi dva pevné disky optimálního.</span><span class="sxs-lookup"><span data-stu-id="8ca3f-139">hello tool will distribute data across two hard drives in an optimized way.</span></span>

## <a name="attach-drives-and-configure-hello-job"></a><span data-ttu-id="8ca3f-140">Připojte jednotky a konfiguraci hello úlohy</span><span class="sxs-lookup"><span data-stu-id="8ca3f-140">Attach drives and configure hello job</span></span>
<span data-ttu-id="8ca3f-141">Bude Připojte oba počítače toohello disky a vytvářet svazky.</span><span class="sxs-lookup"><span data-stu-id="8ca3f-141">You will attach both disks toohello machine and create volumes.</span></span> <span data-ttu-id="8ca3f-142">Pak vytvořte **dataset.csv** souboru:</span><span class="sxs-lookup"><span data-stu-id="8ca3f-142">Then author **dataset.csv** file:</span></span>
```
BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
H:\Video\,video/,BlockBlob,rename,None,H:\mydirectory\properties.xml
H:\Photo\,photo/,BlockBlob,rename,None,H:\mydirectory\properties.xml
K:\Temp\FavoriteVideo.ISO,favorite/FavoriteVideo.ISO,BlockBlob,rename,None,H:\mydirectory\properties.xml
\\myshare\john\music\,music/,BlockBlob,rename,None,H:\mydirectory\properties.xml
```

<span data-ttu-id="8ca3f-143">Kromě toho můžete nastavit hello následující metadata pro všechny soubory:</span><span class="sxs-lookup"><span data-stu-id="8ca3f-143">In addition, you can set hello following metadata for all files:</span></span>

* <span data-ttu-id="8ca3f-144">**UploadMethod:** služby Windows Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="8ca3f-144">**UploadMethod:** Windows Azure Import/Export service</span></span>
* <span data-ttu-id="8ca3f-145">**DataSetName:** SampleData</span><span class="sxs-lookup"><span data-stu-id="8ca3f-145">**DataSetName:** SampleData</span></span>
* <span data-ttu-id="8ca3f-146">**Datum vytvoření:** 10/1/2013</span><span class="sxs-lookup"><span data-stu-id="8ca3f-146">**CreationDate:** 10/1/2013</span></span>

<span data-ttu-id="8ca3f-147">metadata tooset hello importu souborů, vytvořte textový soubor, `c:\WAImportExport\SampleMetadata.txt`, s hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="8ca3f-147">tooset metadata for hello imported files, create a text file, `c:\WAImportExport\SampleMetadata.txt`, with hello following content:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Metadata>
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>
    <DataSetName>SampleData</DataSetName>
    <CreationDate>10/1/2013</CreationDate>
</Metadata>
```

<span data-ttu-id="8ca3f-148">Můžete vytvořit také některé vlastnosti pro hello `FavoriteMovie.ISO` objektů blob:</span><span class="sxs-lookup"><span data-stu-id="8ca3f-148">You can also set some properties for hello `FavoriteMovie.ISO` blob:</span></span>

* <span data-ttu-id="8ca3f-149">**Content-Type:** application/octet-stream</span><span class="sxs-lookup"><span data-stu-id="8ca3f-149">**Content-Type:** application/octet-stream</span></span>
* <span data-ttu-id="8ca3f-150">**Obsah MD5:** Q2hlY2sgSW50ZWdyaXR5IQ ==</span><span class="sxs-lookup"><span data-stu-id="8ca3f-150">**Content-MD5:** Q2hlY2sgSW50ZWdyaXR5IQ==</span></span>
* <span data-ttu-id="8ca3f-151">**Cache-Control:** no cache</span><span class="sxs-lookup"><span data-stu-id="8ca3f-151">**Cache-Control:** no-cache</span></span>

<span data-ttu-id="8ca3f-152">tooset tyto vlastnosti, vytvořte textový soubor, `c:\WAImportExport\SampleProperties.txt`:</span><span class="sxs-lookup"><span data-stu-id="8ca3f-152">tooset these properties, create a text file, `c:\WAImportExport\SampleProperties.txt`:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Properties>
    <Content-Type>application/octet-stream</Content-Type>
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>
    <Cache-Control>no-cache</Cache-Control>
</Properties>
```

## <a name="run-hello-azure-importexport-tool-waimportexportexe"></a><span data-ttu-id="8ca3f-153">Spuštění hello nástroj Azure Import/Export (WAImportExport.exe)</span><span class="sxs-lookup"><span data-stu-id="8ca3f-153">Run hello Azure Import/Export Tool (WAImportExport.exe)</span></span>

<span data-ttu-id="8ca3f-154">Teď je připraven toorun hello nástroj Azure Import/Export tooprepare hello dva pevné disky.</span><span class="sxs-lookup"><span data-stu-id="8ca3f-154">Now you are ready toorun hello Azure Import/Export Tool tooprepare hello two hard drives.</span></span>

<span data-ttu-id="8ca3f-155">**Pro první relaci hello:**</span><span class="sxs-lookup"><span data-stu-id="8ca3f-155">**For hello first session:**</span></span>

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:************* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

<span data-ttu-id="8ca3f-156">Pokud žádná další data potřebuje toobe přidat, vytvořte jiný soubor datovou sadu (stejný formát jako Initialdataset).</span><span class="sxs-lookup"><span data-stu-id="8ca3f-156">If any more data needs toobe added, create another dataset file (same format as Initialdataset).</span></span>

<span data-ttu-id="8ca3f-157">**Pro hello druhý relace:**</span><span class="sxs-lookup"><span data-stu-id="8ca3f-157">**For hello second session:**</span></span>

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
```

<span data-ttu-id="8ca3f-158">Jakmile jste dokončili hello kopírování relací, můžete odpojit hello dvě jednotky z počítače kopie hello a dodávat je toohello příslušné datové centrum Azure.</span><span class="sxs-lookup"><span data-stu-id="8ca3f-158">Once hello copy sessions have completed, you can disconnect hello two drives from hello copy computer and ship them toohello appropriate Azure data center.</span></span> <span data-ttu-id="8ca3f-159">Budete nahrát hello dva soubory deníku, `<FirstDriveSerialNumber>.xml` a `<SecondDriveSerialNumber>.xml`, když vytvoříte hello úlohy importu v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8ca3f-159">You'll upload hello two journal files, `<FirstDriveSerialNumber>.xml` and `<SecondDriveSerialNumber>.xml`, when you create hello import job in hello Azure portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8ca3f-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8ca3f-160">Next steps</span></span>

* [<span data-ttu-id="8ca3f-161">Příprava pevných disků pro úlohu importu</span><span class="sxs-lookup"><span data-stu-id="8ca3f-161">Preparing hard drives for an import job</span></span>](storage-import-export-tool-preparing-hard-drives-import.md)
* [<span data-ttu-id="8ca3f-162">Stručná referenční příručka pro často používané příkazy</span><span class="sxs-lookup"><span data-stu-id="8ca3f-162">Quick reference for frequently used commands</span></span>](storage-import-export-tool-quick-reference.md)
