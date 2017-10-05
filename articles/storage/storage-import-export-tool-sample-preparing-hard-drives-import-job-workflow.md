---
title: "Ukázkový pracovní postup připravená data pevné disky pro úlohy importu Azure Import/Export | Microsoft Docs"
description: "Návod pro dokončení procesu přípravy jednotky pro úlohy importu v rámci služby Azure Import/Export najdete."
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
ms.openlocfilehash: 78d7ce3bbd3205fd995ba331af08d830097c8156
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="sample-workflow-to-prepare-hard-drives-for-an-import-job"></a><span data-ttu-id="e7e1d-103">Ukázkový pracovní postup pro přípravu pevných disků pro úlohu importu</span><span class="sxs-lookup"><span data-stu-id="e7e1d-103">Sample workflow to prepare hard drives for an import job</span></span>

<span data-ttu-id="e7e1d-104">Tento článek vás provede procesem dokončení úlohy importu Příprava jednotky.</span><span class="sxs-lookup"><span data-stu-id="e7e1d-104">This article walks you through the complete process of preparing drives for an import job.</span></span>

## <a name="sample-data"></a><span data-ttu-id="e7e1d-105">Ukázková data</span><span class="sxs-lookup"><span data-stu-id="e7e1d-105">Sample data</span></span>

<span data-ttu-id="e7e1d-106">Tento příklad importuje následující data do účtu úložiště Azure s názvem `mystorageaccount`:</span><span class="sxs-lookup"><span data-stu-id="e7e1d-106">This example imports the following data into an Azure storage account named `mystorageaccount`:</span></span>

|<span data-ttu-id="e7e1d-107">Umístění</span><span class="sxs-lookup"><span data-stu-id="e7e1d-107">Location</span></span>|<span data-ttu-id="e7e1d-108">Popis</span><span class="sxs-lookup"><span data-stu-id="e7e1d-108">Description</span></span>|<span data-ttu-id="e7e1d-109">Velikost dat</span><span class="sxs-lookup"><span data-stu-id="e7e1d-109">Data size</span></span>|
|--------------|-----------------|-----|
|<span data-ttu-id="e7e1d-110">H:\Video\\</span><span class="sxs-lookup"><span data-stu-id="e7e1d-110">H:\Video\\</span></span> |<span data-ttu-id="e7e1d-111">Kolekce videa</span><span class="sxs-lookup"><span data-stu-id="e7e1d-111">A collection of videos</span></span>|<span data-ttu-id="e7e1d-112">12 TB</span><span class="sxs-lookup"><span data-stu-id="e7e1d-112">12 TB</span></span>|
|<span data-ttu-id="e7e1d-113">H:\Photo\\</span><span class="sxs-lookup"><span data-stu-id="e7e1d-113">H:\Photo\\</span></span> |<span data-ttu-id="e7e1d-114">Kolekce fotografií</span><span class="sxs-lookup"><span data-stu-id="e7e1d-114">A collection of photos</span></span>|<span data-ttu-id="e7e1d-115">30 GB</span><span class="sxs-lookup"><span data-stu-id="e7e1d-115">30 GB</span></span>|
|<span data-ttu-id="e7e1d-116">K:\Temp\FavoriteMovie.ISO</span><span class="sxs-lookup"><span data-stu-id="e7e1d-116">K:\Temp\FavoriteMovie.ISO</span></span>|<span data-ttu-id="e7e1d-117">Bitová kopie disku A Blu-ray™</span><span class="sxs-lookup"><span data-stu-id="e7e1d-117">A Blu-Ray™ disk image</span></span>|<span data-ttu-id="e7e1d-118">25 GB</span><span class="sxs-lookup"><span data-stu-id="e7e1d-118">25 GB</span></span>|
|<span data-ttu-id="e7e1d-119">\\\bigshare\john\music\\</span><span class="sxs-lookup"><span data-stu-id="e7e1d-119">\\\bigshare\john\music\\</span></span>|<span data-ttu-id="e7e1d-120">Kolekce hudebních souborů ve sdílené síťové složce</span><span class="sxs-lookup"><span data-stu-id="e7e1d-120">A collection of music files on a network share</span></span>|<span data-ttu-id="e7e1d-121">10 GB</span><span class="sxs-lookup"><span data-stu-id="e7e1d-121">10 GB</span></span>|

## <a name="storage-account-destinations"></a><span data-ttu-id="e7e1d-122">Cíle účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="e7e1d-122">Storage account destinations</span></span>

<span data-ttu-id="e7e1d-123">Úloha importu bude import dat do následující cíle v účtu úložiště:</span><span class="sxs-lookup"><span data-stu-id="e7e1d-123">The import job will import the data into the following destinations in the storage account:</span></span>

|<span data-ttu-id="e7e1d-124">Zdroj</span><span class="sxs-lookup"><span data-stu-id="e7e1d-124">Source</span></span>|<span data-ttu-id="e7e1d-125">Cílový virtuální adresář nebo objekt blob</span><span class="sxs-lookup"><span data-stu-id="e7e1d-125">Destination virtual directory or blob</span></span>|
|------------|-------------------------------------------|
|<span data-ttu-id="e7e1d-126">H:\Video\\</span><span class="sxs-lookup"><span data-stu-id="e7e1d-126">H:\Video\\</span></span> |<span data-ttu-id="e7e1d-127">video nebo</span><span class="sxs-lookup"><span data-stu-id="e7e1d-127">video/</span></span>|
|<span data-ttu-id="e7e1d-128">H:\Photo\\</span><span class="sxs-lookup"><span data-stu-id="e7e1d-128">H:\Photo\\</span></span> |<span data-ttu-id="e7e1d-129">fotografie nebo</span><span class="sxs-lookup"><span data-stu-id="e7e1d-129">photo/</span></span>|
|<span data-ttu-id="e7e1d-130">K:\Temp\FavoriteMovie.ISO</span><span class="sxs-lookup"><span data-stu-id="e7e1d-130">K:\Temp\FavoriteMovie.ISO</span></span>|<span data-ttu-id="e7e1d-131">favorite/FavoriteMovies.ISO</span><span class="sxs-lookup"><span data-stu-id="e7e1d-131">favorite/FavoriteMovies.ISO</span></span>|
|<span data-ttu-id="e7e1d-132">\\\bigshare\john\music\\</span><span class="sxs-lookup"><span data-stu-id="e7e1d-132">\\\bigshare\john\music\\</span></span> |<span data-ttu-id="e7e1d-133">Hudba</span><span class="sxs-lookup"><span data-stu-id="e7e1d-133">music</span></span>|

<span data-ttu-id="e7e1d-134">Pomocí této mapování, souboru `H:\Video\Drama\GreatMovie.mov` budou importovány do objektu blob `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.</span><span class="sxs-lookup"><span data-stu-id="e7e1d-134">With this mapping, the file `H:\Video\Drama\GreatMovie.mov` will be imported to the blob `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.</span></span>

## <a name="determine-hard-drive-requirements"></a><span data-ttu-id="e7e1d-135">Stanovení požadavků na pevném disku</span><span class="sxs-lookup"><span data-stu-id="e7e1d-135">Determine hard drive requirements</span></span>

<span data-ttu-id="e7e1d-136">Dále určit, kolik pevné disky jsou potřeba, Vypočítat velikost dat:</span><span class="sxs-lookup"><span data-stu-id="e7e1d-136">Next, to determine how many hard drives are needed, compute the size of the data:</span></span>

`12TB + 30GB + 25GB + 10GB = 12TB + 65GB`

<span data-ttu-id="e7e1d-137">V tomto příkladu by mělo být dostatečné dva 8TB pevné disky.</span><span class="sxs-lookup"><span data-stu-id="e7e1d-137">For this example, two 8TB hard drives should be sufficient.</span></span> <span data-ttu-id="e7e1d-138">Nicméně, protože zdrojový adresář `H:\Video` má 12TB dat a jeden pevný disk je kapacita je pouze 8TB, bude moci určit to v následujícím způsobem v **driveset.csv** souboru:</span><span class="sxs-lookup"><span data-stu-id="e7e1d-138">However, since the source directory `H:\Video` has 12TB of data and your single hard drive's capacity is only 8TB, you will be able to specify this in the following way in the **driveset.csv** file:</span></span>

```
DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
X,Format,SilentMode,Encrypt,
Y,Format,SilentMode,Encrypt,
```
<span data-ttu-id="e7e1d-139">Nástroj provede distribuci dat mezi dva pevné disky optimálního.</span><span class="sxs-lookup"><span data-stu-id="e7e1d-139">The tool will distribute data across two hard drives in an optimized way.</span></span>

## <a name="attach-drives-and-configure-the-job"></a><span data-ttu-id="e7e1d-140">Připojte jednotky a konfiguraci úlohy</span><span class="sxs-lookup"><span data-stu-id="e7e1d-140">Attach drives and configure the job</span></span>
<span data-ttu-id="e7e1d-141">Můžete se připojit k počítači, oba disky a vytvářet svazky.</span><span class="sxs-lookup"><span data-stu-id="e7e1d-141">You will attach both disks to the machine and create volumes.</span></span> <span data-ttu-id="e7e1d-142">Pak vytvořte **dataset.csv** souboru:</span><span class="sxs-lookup"><span data-stu-id="e7e1d-142">Then author **dataset.csv** file:</span></span>
```
BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
H:\Video\,video/,BlockBlob,rename,None,H:\mydirectory\properties.xml
H:\Photo\,photo/,BlockBlob,rename,None,H:\mydirectory\properties.xml
K:\Temp\FavoriteVideo.ISO,favorite/FavoriteVideo.ISO,BlockBlob,rename,None,H:\mydirectory\properties.xml
\\myshare\john\music\,music/,BlockBlob,rename,None,H:\mydirectory\properties.xml
```

<span data-ttu-id="e7e1d-143">Kromě toho můžete nastavit následující metadata pro všechny soubory:</span><span class="sxs-lookup"><span data-stu-id="e7e1d-143">In addition, you can set the following metadata for all files:</span></span>

* <span data-ttu-id="e7e1d-144">**UploadMethod:** služby Windows Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="e7e1d-144">**UploadMethod:** Windows Azure Import/Export service</span></span>
* <span data-ttu-id="e7e1d-145">**DataSetName:** SampleData</span><span class="sxs-lookup"><span data-stu-id="e7e1d-145">**DataSetName:** SampleData</span></span>
* <span data-ttu-id="e7e1d-146">**Datum vytvoření:** 10/1/2013</span><span class="sxs-lookup"><span data-stu-id="e7e1d-146">**CreationDate:** 10/1/2013</span></span>

<span data-ttu-id="e7e1d-147">Pokud chcete nastavit metadata pro importované soubory, vytvořte textový soubor, `c:\WAImportExport\SampleMetadata.txt`, s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="e7e1d-147">To set metadata for the imported files, create a text file, `c:\WAImportExport\SampleMetadata.txt`, with the following content:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Metadata>
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>
    <DataSetName>SampleData</DataSetName>
    <CreationDate>10/1/2013</CreationDate>
</Metadata>
```

<span data-ttu-id="e7e1d-148">Můžete vytvořit také některé vlastnosti `FavoriteMovie.ISO` objektů blob:</span><span class="sxs-lookup"><span data-stu-id="e7e1d-148">You can also set some properties for the `FavoriteMovie.ISO` blob:</span></span>

* <span data-ttu-id="e7e1d-149">**Content-Type:** application/octet-stream</span><span class="sxs-lookup"><span data-stu-id="e7e1d-149">**Content-Type:** application/octet-stream</span></span>
* <span data-ttu-id="e7e1d-150">**Obsah MD5:** Q2hlY2sgSW50ZWdyaXR5IQ ==</span><span class="sxs-lookup"><span data-stu-id="e7e1d-150">**Content-MD5:** Q2hlY2sgSW50ZWdyaXR5IQ==</span></span>
* <span data-ttu-id="e7e1d-151">**Cache-Control:** no cache</span><span class="sxs-lookup"><span data-stu-id="e7e1d-151">**Cache-Control:** no-cache</span></span>

<span data-ttu-id="e7e1d-152">Chcete-li nastavit tyto vlastnosti, vytvořte textový soubor, `c:\WAImportExport\SampleProperties.txt`:</span><span class="sxs-lookup"><span data-stu-id="e7e1d-152">To set these properties, create a text file, `c:\WAImportExport\SampleProperties.txt`:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Properties>
    <Content-Type>application/octet-stream</Content-Type>
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>
    <Cache-Control>no-cache</Cache-Control>
</Properties>
```

## <a name="run-the-azure-importexport-tool-waimportexportexe"></a><span data-ttu-id="e7e1d-153">Spusťte nástroj Azure Import/Export (WAImportExport.exe)</span><span class="sxs-lookup"><span data-stu-id="e7e1d-153">Run the Azure Import/Export Tool (WAImportExport.exe)</span></span>

<span data-ttu-id="e7e1d-154">Nyní jste připraveni ke spuštění nástroje Azure Import/Export Příprava dva pevné disky.</span><span class="sxs-lookup"><span data-stu-id="e7e1d-154">Now you are ready to run the Azure Import/Export Tool to prepare the two hard drives.</span></span>

<span data-ttu-id="e7e1d-155">**Pro první relaci:**</span><span class="sxs-lookup"><span data-stu-id="e7e1d-155">**For the first session:**</span></span>

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:************* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

<span data-ttu-id="e7e1d-156">Pokud žádná další data musí být přidán, vytvořte jiný soubor datovou sadu (stejný formát jako Initialdataset).</span><span class="sxs-lookup"><span data-stu-id="e7e1d-156">If any more data needs to be added, create another dataset file (same format as Initialdataset).</span></span>

<span data-ttu-id="e7e1d-157">**Pro druhou relaci:**</span><span class="sxs-lookup"><span data-stu-id="e7e1d-157">**For the second session:**</span></span>

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
```

<span data-ttu-id="e7e1d-158">Po dokončení kopírování relací mají můžete odpojit dvě jednotky z počítače, kopírování a dodávat je k příslušné datového centra Azure.</span><span class="sxs-lookup"><span data-stu-id="e7e1d-158">Once the copy sessions have completed, you can disconnect the two drives from the copy computer and ship them to the appropriate Azure data center.</span></span> <span data-ttu-id="e7e1d-159">Budete odesílat soubory, dvě deníku, `<FirstDriveSerialNumber>.xml` a `<SecondDriveSerialNumber>.xml`, při vytváření úlohy importu v portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e7e1d-159">You'll upload the two journal files, `<FirstDriveSerialNumber>.xml` and `<SecondDriveSerialNumber>.xml`, when you create the import job in the Azure portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e7e1d-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e7e1d-160">Next steps</span></span>

* [<span data-ttu-id="e7e1d-161">Příprava pevných disků pro úlohu importu</span><span class="sxs-lookup"><span data-stu-id="e7e1d-161">Preparing hard drives for an import job</span></span>](storage-import-export-tool-preparing-hard-drives-import.md)
* [<span data-ttu-id="e7e1d-162">Stručná referenční příručka pro často používané příkazy</span><span class="sxs-lookup"><span data-stu-id="e7e1d-162">Quick reference for frequently used commands</span></span>](storage-import-export-tool-quick-reference.md)
