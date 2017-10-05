---
title: "Ukázkový pracovní postup připravená data pevné disky pro úlohy importu Azure Import/Export - v1 | Microsoft Docs"
description: "Návod pro dokončení procesu přípravy jednotky pro úlohy importu v rámci služby Azure Import/Export najdete."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 6eb1b1b7-c69f-4365-b5ef-3cd5e05eb72a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 179c6bac9a2d9509baa0007a7008d75d0874a25e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="sample-workflow-to-prepare-hard-drives-for-an-import-job"></a><span data-ttu-id="2ed1a-103">Ukázkový pracovní postup pro přípravu pevných disků pro úlohu importu</span><span class="sxs-lookup"><span data-stu-id="2ed1a-103">Sample workflow to prepare hard drives for an import job</span></span>
<span data-ttu-id="2ed1a-104">Toto téma vás provede kompletní proces přípravy jednotky pro úlohy importu.</span><span class="sxs-lookup"><span data-stu-id="2ed1a-104">This topic walks you through the complete process of preparing drives for an import job.</span></span>  
  
<span data-ttu-id="2ed1a-105">Tento příklad importuje následující data do účtu úložiště Azure okno s názvem `mystorageaccount`:</span><span class="sxs-lookup"><span data-stu-id="2ed1a-105">This example imports the following data into a Window Azure storage account named `mystorageaccount`:</span></span>  
  
|<span data-ttu-id="2ed1a-106">Umístění</span><span class="sxs-lookup"><span data-stu-id="2ed1a-106">Location</span></span>|<span data-ttu-id="2ed1a-107">Popis</span><span class="sxs-lookup"><span data-stu-id="2ed1a-107">Description</span></span>|  
|--------------|-----------------|  
|<span data-ttu-id="2ed1a-108">H:\Video</span><span class="sxs-lookup"><span data-stu-id="2ed1a-108">H:\Video</span></span>|<span data-ttu-id="2ed1a-109">Kolekce videa, 5 TB celkem.</span><span class="sxs-lookup"><span data-stu-id="2ed1a-109">A collection of videos, 5 TB in total.</span></span>|  
|<span data-ttu-id="2ed1a-110">H:\Photo</span><span class="sxs-lookup"><span data-stu-id="2ed1a-110">H:\Photo</span></span>|<span data-ttu-id="2ed1a-111">Kolekce fotografií, celkem 30 GB.</span><span class="sxs-lookup"><span data-stu-id="2ed1a-111">A collection of photos, 30 GB in total.</span></span>|  
|<span data-ttu-id="2ed1a-112">K:\Temp\FavoriteMovie.ISO</span><span class="sxs-lookup"><span data-stu-id="2ed1a-112">K:\Temp\FavoriteMovie.ISO</span></span>|<span data-ttu-id="2ed1a-113">Image disku A Blu-ray™, 25 GB.</span><span class="sxs-lookup"><span data-stu-id="2ed1a-113">A Blu-Ray™ disk image, 25 GB.</span></span>|  
|<span data-ttu-id="2ed1a-114">\\\bigshare\john\music</span><span class="sxs-lookup"><span data-stu-id="2ed1a-114">\\\bigshare\john\music</span></span>|<span data-ttu-id="2ed1a-115">Kolekce hudebních souborů ve sdílené síťové složce, celkem 10 GB.</span><span class="sxs-lookup"><span data-stu-id="2ed1a-115">A collection of music files on a network share, 10 GB in total.</span></span>|  
  
<span data-ttu-id="2ed1a-116">Úloha importu importuje tato data do následující cíle v účtu úložiště:</span><span class="sxs-lookup"><span data-stu-id="2ed1a-116">The import job imports this data into the following destinations in the storage account:</span></span>  
  
|<span data-ttu-id="2ed1a-117">Zdroj</span><span class="sxs-lookup"><span data-stu-id="2ed1a-117">Source</span></span>|<span data-ttu-id="2ed1a-118">Cílový virtuální adresář nebo objekt blob</span><span class="sxs-lookup"><span data-stu-id="2ed1a-118">Destination virtual directory or blob</span></span>|  
|------------|-------------------------------------------|  
|<span data-ttu-id="2ed1a-119">H:\Video</span><span class="sxs-lookup"><span data-stu-id="2ed1a-119">H:\Video</span></span>|<span data-ttu-id="2ed1a-120">https://mystorageaccount.BLOB.Core.Windows.NET/video</span><span class="sxs-lookup"><span data-stu-id="2ed1a-120">https://mystorageaccount.blob.core.windows.net/video</span></span>|  
|<span data-ttu-id="2ed1a-121">H:\Photo</span><span class="sxs-lookup"><span data-stu-id="2ed1a-121">H:\Photo</span></span>|<span data-ttu-id="2ed1a-122">https://mystorageaccount.BLOB.Core.Windows.NET/Photo</span><span class="sxs-lookup"><span data-stu-id="2ed1a-122">https://mystorageaccount.blob.core.windows.net/photo</span></span>|  
|<span data-ttu-id="2ed1a-123">K:\Temp\FavoriteMovie.ISO</span><span class="sxs-lookup"><span data-stu-id="2ed1a-123">K:\Temp\FavoriteMovie.ISO</span></span>|<span data-ttu-id="2ed1a-124">https://mystorageaccount.BLOB.Core.Windows.NET/Favorite/FavoriteMovies.ISO</span><span class="sxs-lookup"><span data-stu-id="2ed1a-124">https://mystorageaccount.blob.core.windows.net/favorite/FavoriteMovies.ISO</span></span>|  
|<span data-ttu-id="2ed1a-125">\\\bigshare\john\music</span><span class="sxs-lookup"><span data-stu-id="2ed1a-125">\\\bigshare\john\music</span></span>|<span data-ttu-id="2ed1a-126">https://mystorageaccount.BLOB.Core.Windows.NET/Music</span><span class="sxs-lookup"><span data-stu-id="2ed1a-126">https://mystorageaccount.blob.core.windows.net/music</span></span>|  
  
<span data-ttu-id="2ed1a-127">Pomocí této mapování, souboru `H:\Video\Drama\GreatMovie.mov` je importovat do objektu blob `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.</span><span class="sxs-lookup"><span data-stu-id="2ed1a-127">With this mapping, the file `H:\Video\Drama\GreatMovie.mov` is imported to the blob `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.</span></span>  
  
<span data-ttu-id="2ed1a-128">Dále určit, kolik pevné disky jsou potřeba, Vypočítat velikost dat:</span><span class="sxs-lookup"><span data-stu-id="2ed1a-128">Next, to determine how many hard drives are needed, compute the size of the data:</span></span>  
  
`5TB + 30GB + 25GB + 10GB = 5TB + 65GB`  
  
<span data-ttu-id="2ed1a-129">V tomto příkladu by mělo být dostatečné dva 3 TB pevné disky.</span><span class="sxs-lookup"><span data-stu-id="2ed1a-129">For this example, two 3-TB hard drives should be sufficient.</span></span> <span data-ttu-id="2ed1a-130">Nicméně, protože zdrojový adresář `H:\Video` má 5 TB dat a jeden pevný disk je kapacita je jenom 3 TB, je nutné rozdělit `H:\Video` do dvou menší adresářů: `H:\Video1` a `H:\Video2`, před spuštěním Microsoft Azure Nástroj pro import nebo Export.</span><span class="sxs-lookup"><span data-stu-id="2ed1a-130">However, since the source directory `H:\Video` has 5 TB of data and your single hard drive's capacity is only 3 TB, it's necessary to break `H:\Video` into two smaller directories: `H:\Video1` and `H:\Video2`, before running the Microsoft Azure Import/Export Tool.</span></span> <span data-ttu-id="2ed1a-131">Tento krok vypočítá následující zdrojového adresáře:</span><span class="sxs-lookup"><span data-stu-id="2ed1a-131">This step yields the following source directories:</span></span>  
  
|<span data-ttu-id="2ed1a-132">Umístění</span><span class="sxs-lookup"><span data-stu-id="2ed1a-132">Location</span></span>|<span data-ttu-id="2ed1a-133">Velikost</span><span class="sxs-lookup"><span data-stu-id="2ed1a-133">Size</span></span>|<span data-ttu-id="2ed1a-134">Cílový virtuální adresář nebo objekt blob</span><span class="sxs-lookup"><span data-stu-id="2ed1a-134">Destination virtual directory or blob</span></span>|  
|--------------|----------|-------------------------------------------|  
|<span data-ttu-id="2ed1a-135">H:\Video1</span><span class="sxs-lookup"><span data-stu-id="2ed1a-135">H:\Video1</span></span>|<span data-ttu-id="2ed1a-136">2,5 TB</span><span class="sxs-lookup"><span data-stu-id="2ed1a-136">2.5 TB</span></span>|<span data-ttu-id="2ed1a-137">https://mystorageaccount.BLOB.Core.Windows.NET/video</span><span class="sxs-lookup"><span data-stu-id="2ed1a-137">https://mystorageaccount.blob.core.windows.net/video</span></span>|  
|<span data-ttu-id="2ed1a-138">H:\Video2</span><span class="sxs-lookup"><span data-stu-id="2ed1a-138">H:\Video2</span></span>|<span data-ttu-id="2ed1a-139">2,5 TB</span><span class="sxs-lookup"><span data-stu-id="2ed1a-139">2.5 TB</span></span>|<span data-ttu-id="2ed1a-140">https://mystorageaccount.BLOB.Core.Windows.NET/video</span><span class="sxs-lookup"><span data-stu-id="2ed1a-140">https://mystorageaccount.blob.core.windows.net/video</span></span>|  
|<span data-ttu-id="2ed1a-141">H:\Photo</span><span class="sxs-lookup"><span data-stu-id="2ed1a-141">H:\Photo</span></span>|<span data-ttu-id="2ed1a-142">30 GB</span><span class="sxs-lookup"><span data-stu-id="2ed1a-142">30 GB</span></span>|<span data-ttu-id="2ed1a-143">https://mystorageaccount.BLOB.Core.Windows.NET/Photo</span><span class="sxs-lookup"><span data-stu-id="2ed1a-143">https://mystorageaccount.blob.core.windows.net/photo</span></span>|  
|<span data-ttu-id="2ed1a-144">K:\Temp\FavoriteMovies.ISO</span><span class="sxs-lookup"><span data-stu-id="2ed1a-144">K:\Temp\FavoriteMovies.ISO</span></span>|<span data-ttu-id="2ed1a-145">25 GB</span><span class="sxs-lookup"><span data-stu-id="2ed1a-145">25 GB</span></span>|<span data-ttu-id="2ed1a-146">https://mystorageaccount.BLOB.Core.Windows.NET/Favorite/FavoriteMovies.ISO</span><span class="sxs-lookup"><span data-stu-id="2ed1a-146">https://mystorageaccount.blob.core.windows.net/favorite/FavoriteMovies.ISO</span></span>|  
|<span data-ttu-id="2ed1a-147">\\\bigshare\john\music</span><span class="sxs-lookup"><span data-stu-id="2ed1a-147">\\\bigshare\john\music</span></span>|<span data-ttu-id="2ed1a-148">10 GB</span><span class="sxs-lookup"><span data-stu-id="2ed1a-148">10 GB</span></span>|<span data-ttu-id="2ed1a-149">https://mystorageaccount.BLOB.Core.Windows.NET/Music</span><span class="sxs-lookup"><span data-stu-id="2ed1a-149">https://mystorageaccount.blob.core.windows.net/music</span></span>|  
  
 <span data-ttu-id="2ed1a-150">I když `H:\Video`directory rozdělení na dva adresáře ukazovaly na stejný cílový virtuální adresář v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="2ed1a-150">Even though the `H:\Video`directory has been split to two directories, they point to the same destination virtual directory in the storage account.</span></span> <span data-ttu-id="2ed1a-151">Tímto způsobem, všechny soubory videa se udržují v rámci jedné `video` kontejneru v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="2ed1a-151">This way, all video files are maintained under a single `video` container in the storage account.</span></span>  
  
 <span data-ttu-id="2ed1a-152">V dalším kroku předchozí zdrojového adresáře jsou rovnoměrně rozloženy dva pevné disky:</span><span class="sxs-lookup"><span data-stu-id="2ed1a-152">Next, the previous source directories are evenly distributed to the two hard drives:</span></span>  
  
||||  
|-|-|-|  
|<span data-ttu-id="2ed1a-153">Pevný disk</span><span class="sxs-lookup"><span data-stu-id="2ed1a-153">Hard drive</span></span>|<span data-ttu-id="2ed1a-154">Zdrojové adresáře</span><span class="sxs-lookup"><span data-stu-id="2ed1a-154">Source directories</span></span>|<span data-ttu-id="2ed1a-155">Celková velikost</span><span class="sxs-lookup"><span data-stu-id="2ed1a-155">Total size</span></span>|  
|<span data-ttu-id="2ed1a-156">První jednotky</span><span class="sxs-lookup"><span data-stu-id="2ed1a-156">First Drive</span></span>|<span data-ttu-id="2ed1a-157">H:\Video1</span><span class="sxs-lookup"><span data-stu-id="2ed1a-157">H:\Video1</span></span>|<span data-ttu-id="2ed1a-158">2,5 TB + 30 GB</span><span class="sxs-lookup"><span data-stu-id="2ed1a-158">2.5 TB + 30 GB</span></span>|  
||<span data-ttu-id="2ed1a-159">H:\Photo</span><span class="sxs-lookup"><span data-stu-id="2ed1a-159">H:\Photo</span></span>||  
|<span data-ttu-id="2ed1a-160">Sekundy jednotky</span><span class="sxs-lookup"><span data-stu-id="2ed1a-160">Second Drive</span></span>|<span data-ttu-id="2ed1a-161">H:\Video2</span><span class="sxs-lookup"><span data-stu-id="2ed1a-161">H:\Video2</span></span>|<span data-ttu-id="2ed1a-162">2,5 TB + 35 GB</span><span class="sxs-lookup"><span data-stu-id="2ed1a-162">2.5 TB + 35 GB</span></span>|  
||<span data-ttu-id="2ed1a-163">K:\Temp\BlueRay.ISO</span><span class="sxs-lookup"><span data-stu-id="2ed1a-163">K:\Temp\BlueRay.ISO</span></span>||  
||<span data-ttu-id="2ed1a-164">\\\bigshare\john\music</span><span class="sxs-lookup"><span data-stu-id="2ed1a-164">\\\bigshare\john\music</span></span>||  
  
<span data-ttu-id="2ed1a-165">Kromě toho můžete nastavit následující metadata pro všechny soubory:</span><span class="sxs-lookup"><span data-stu-id="2ed1a-165">In addition, you can set the following metadata for all files:</span></span>  
  
-   <span data-ttu-id="2ed1a-166">**UploadMethod:** služby Windows Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="2ed1a-166">**UploadMethod:** Windows Azure Import/Export service</span></span>  
  
-   <span data-ttu-id="2ed1a-167">**DataSetName:** SampleData</span><span class="sxs-lookup"><span data-stu-id="2ed1a-167">**DataSetName:** SampleData</span></span>  
  
-   <span data-ttu-id="2ed1a-168">**Datum vytvoření:** 10/1/2013</span><span class="sxs-lookup"><span data-stu-id="2ed1a-168">**CreationDate:** 10/1/2013</span></span>  
  
<span data-ttu-id="2ed1a-169">Pokud chcete nastavit metadata pro importované soubory, vytvořte textový soubor, `c:\WAImportExport\SampleMetadata.txt`, s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="2ed1a-169">To set metadata for the imported files, create a text file, `c:\WAImportExport\SampleMetadata.txt`, with the following content:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>  
    <DataSetName>SampleData</DataSetName>  
    <CreationDate>10/1/2013</CreationDate>  
</Metadata>  
```
  
<span data-ttu-id="2ed1a-170">Můžete vytvořit také některé vlastnosti `FavoriteMovie.ISO` objektů blob:</span><span class="sxs-lookup"><span data-stu-id="2ed1a-170">You can also set some properties for the `FavoriteMovie.ISO` blob:</span></span>  
  
-   <span data-ttu-id="2ed1a-171">**Content-Type:** application/octet-stream</span><span class="sxs-lookup"><span data-stu-id="2ed1a-171">**Content-Type:** application/octet-stream</span></span>  
  
-   <span data-ttu-id="2ed1a-172">**Obsah MD5:** Q2hlY2sgSW50ZWdyaXR5IQ ==</span><span class="sxs-lookup"><span data-stu-id="2ed1a-172">**Content-MD5:** Q2hlY2sgSW50ZWdyaXR5IQ==</span></span>  
  
-   <span data-ttu-id="2ed1a-173">**Cache-Control:** no cache</span><span class="sxs-lookup"><span data-stu-id="2ed1a-173">**Cache-Control:** no-cache</span></span>  
  
<span data-ttu-id="2ed1a-174">Chcete-li nastavit tyto vlastnosti, vytvořte textový soubor, `c:\WAImportExport\SampleProperties.txt`:</span><span class="sxs-lookup"><span data-stu-id="2ed1a-174">To set these properties, create a text file, `c:\WAImportExport\SampleProperties.txt`:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
    <Content-Type>application/octet-stream</Content-Type>  
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>  
    <Cache-Control>no-cache</Cache-Control>  
</Properties>  
```
  
<span data-ttu-id="2ed1a-175">Nyní jste připraveni ke spuštění nástroje Azure Import/Export Příprava dva pevné disky.</span><span class="sxs-lookup"><span data-stu-id="2ed1a-175">Now you are ready to run the Azure Import/Export Tool to prepare the two hard drives.</span></span> <span data-ttu-id="2ed1a-176">Poznámky:</span><span class="sxs-lookup"><span data-stu-id="2ed1a-176">Note that:</span></span>  
  
-   <span data-ttu-id="2ed1a-177">První disk je připojit jako jednotka X.</span><span class="sxs-lookup"><span data-stu-id="2ed1a-177">The first drive is mounted as drive X.</span></span>  
  
-   <span data-ttu-id="2ed1a-178">Druhý disk je připojit jako disk Y.</span><span class="sxs-lookup"><span data-stu-id="2ed1a-178">The second drive is mounted as drive Y.</span></span>  
  
-   <span data-ttu-id="2ed1a-179">Klíč pro účet úložiště `mystorageaccount` je `8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg==`.</span><span class="sxs-lookup"><span data-stu-id="2ed1a-179">The key for the storage account `mystorageaccount` is `8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg==`.</span></span>  

## <a name="preparing-disk-for-import-when-data-is-pre-loaded"></a><span data-ttu-id="2ed1a-180">Příprava disku pro import, když je předem načtená data</span><span class="sxs-lookup"><span data-stu-id="2ed1a-180">Preparing disk for import when data is pre-loaded</span></span>
 
 <span data-ttu-id="2ed1a-181">Pokud data, která mají být importována již existuje na disku, použijte příznak /skipwrite.</span><span class="sxs-lookup"><span data-stu-id="2ed1a-181">If the data to be imported is already present on the disk, use the flag /skipwrite.</span></span> <span data-ttu-id="2ed1a-182">Hodnota /t a /srcdir má obě, přejděte na disku připraveném pro import.</span><span class="sxs-lookup"><span data-stu-id="2ed1a-182">The value of /t and /srcdir should both point to the disk being prepared for import.</span></span> <span data-ttu-id="2ed1a-183">Pokud všechna data, která bude importována není má stejné cílový virtuální adresář nebo kořenového účtu úložiště, spusťte stejný příkaz pro každý cílový adresář samostatně, udržování hodnota /id stejné napříč všechny spustí.</span><span class="sxs-lookup"><span data-stu-id="2ed1a-183">If all of the data to be imported is not going to the same destination virtual directory or root of the storage account, run the same command for each destination directory separately, keeping the value of /id the same across all runs.</span></span>

>[!NOTE] 
><span data-ttu-id="2ed1a-184">Nezadávejte/Format, jak se bude vymazání dat na disku.</span><span class="sxs-lookup"><span data-stu-id="2ed1a-184">Do not specify /format as it will wipe the data on the disk.</span></span> <span data-ttu-id="2ed1a-185">Můžete zadat / šifrování nebo /bk v závislosti na tom, jestli je disk už je šifrovaný nebo ne.</span><span class="sxs-lookup"><span data-stu-id="2ed1a-185">You can specify /encrypt or /bk depending on whether the disk is already encrypted or not.</span></span> 
>

```
    When data is already present on the disk for each drive run the following command.
    WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:x:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt /skipwrite
```

## <a name="copy-sessions---first-drive"></a><span data-ttu-id="2ed1a-186">Zkopírujte relací - nejprve jednotky</span><span class="sxs-lookup"><span data-stu-id="2ed1a-186">Copy sessions - first drive</span></span>

<span data-ttu-id="2ed1a-187">Pro první disk spusťte nástroj Azure Import/Export dvakrát pro kopírování adresářů dva zdroje:</span><span class="sxs-lookup"><span data-stu-id="2ed1a-187">For the first drive, run the Azure Import/Export Tool twice to copy the two source directories:</span></span>  

<span data-ttu-id="2ed1a-188">**Nejdříve zkopírovat relace**</span><span class="sxs-lookup"><span data-stu-id="2ed1a-188">**First copy session**</span></span>
  
```
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:H:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```

<span data-ttu-id="2ed1a-189">**Druhá kopie relace**</span><span class="sxs-lookup"><span data-stu-id="2ed1a-189">**Second copy session**</span></span>

```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Photo /srcdir:H:\Photo /dstdir:photo/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt
```

## <a name="copy-sessions---second-drive"></a><span data-ttu-id="2ed1a-190">Zkopírujte relací - druhé jednotky</span><span class="sxs-lookup"><span data-stu-id="2ed1a-190">Copy sessions - second drive</span></span>
 
<span data-ttu-id="2ed1a-191">Pro druhý disk, spusťte nástroj Azure Import/Export trojnásobek, jednou každý pro zdrojového adresáře a jednou pro soubor bitové kopie samostatné Blu-Ray™):</span><span class="sxs-lookup"><span data-stu-id="2ed1a-191">For the second drive, run the Azure Import/Export Tool three times, once each for the source directories, and once for the standalone Blu-Ray™ image file):</span></span>  
  
<span data-ttu-id="2ed1a-192">**Nejdříve zkopírovat relace**</span><span class="sxs-lookup"><span data-stu-id="2ed1a-192">**First copy session**</span></span> 

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:Video2 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:y /format /encrypt /srcdir:H:\Video2 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```
  
<span data-ttu-id="2ed1a-193">**Druhá kopie relace**</span><span class="sxs-lookup"><span data-stu-id="2ed1a-193">**Second copy session**</span></span>

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:Music /srcdir:\\bigshare\john\music /dstdir:music/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```  
  
<span data-ttu-id="2ed1a-194">**Třetí kopii relace**</span><span class="sxs-lookup"><span data-stu-id="2ed1a-194">**Third copy session**</span></span>  

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:BlueRayIso /srcfile:K:\Temp\BlueRay.ISO /dstblob:favorite/BlueRay.ISO /MetadataFile:c:\WAImportExport\SampleMetadata.txt /PropertyFile:c:\WAImportExport\SampleProperties.txt  
```

## <a name="copy-session-completion"></a><span data-ttu-id="2ed1a-195">Zkopírujte dokončení relace</span><span class="sxs-lookup"><span data-stu-id="2ed1a-195">Copy session completion</span></span>

<span data-ttu-id="2ed1a-196">Po dokončení kopírování relací mají můžete odpojit dvě jednotky z počítače, kopírování a dodávat je k příslušné datovému centru systému Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="2ed1a-196">Once the copy sessions have completed, you can disconnect the two drives from the copy computer and ship them to the appropriate Windows Azure data center.</span></span> <span data-ttu-id="2ed1a-197">Odesílat soubory dvě deníku `FirstDrive.jrn` a `SecondDrive.jrn`, při vytváření úlohy importu v [portálu Windows Azure](https://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="2ed1a-197">Upload the two journal files, `FirstDrive.jrn` and `SecondDrive.jrn`, when you create the import job in the [Windows Azure portal](https://manage.windowsazure.com/).</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="2ed1a-198">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2ed1a-198">Next steps</span></span>

* [<span data-ttu-id="2ed1a-199">Příprava pevných disků pro úlohu importu</span><span class="sxs-lookup"><span data-stu-id="2ed1a-199">Preparing hard drives for an import job</span></span>](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [<span data-ttu-id="2ed1a-200">Stručná referenční příručka pro často používané příkazy</span><span class="sxs-lookup"><span data-stu-id="2ed1a-200">Quick reference for frequently used commands</span></span>](../storage-import-export-tool-quick-reference-v1.md) 
