---
title: "aaaSample pracovního postupu tooprep pevné disky pro Azure importu a exportu importovat úlohy - v1 | Microsoft Docs"
description: "Návod pro dokončení procesu hello přípravy jednotky pro úlohy importu v hello služba Azure Import/Export najdete."
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
ms.openlocfilehash: eb77831a88c16c14838179a6432ddb06503067dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sample-workflow-tooprepare-hard-drives-for-an-import-job"></a><span data-ttu-id="07977-103">Ukázkový pracovní postup tooprepare pevné disky pro úlohy importu</span><span class="sxs-lookup"><span data-stu-id="07977-103">Sample workflow tooprepare hard drives for an import job</span></span>
<span data-ttu-id="07977-104">Toto téma vás provede procesem hello dokončení procesu přípravy jednotky pro úlohy importu.</span><span class="sxs-lookup"><span data-stu-id="07977-104">This topic walks you through hello complete process of preparing drives for an import job.</span></span>  
  
<span data-ttu-id="07977-105">Tento příklad importuje následující data do účtu úložiště Azure okno s názvem hello `mystorageaccount`:</span><span class="sxs-lookup"><span data-stu-id="07977-105">This example imports hello following data into a Window Azure storage account named `mystorageaccount`:</span></span>  
  
|<span data-ttu-id="07977-106">Umístění</span><span class="sxs-lookup"><span data-stu-id="07977-106">Location</span></span>|<span data-ttu-id="07977-107">Popis</span><span class="sxs-lookup"><span data-stu-id="07977-107">Description</span></span>|  
|--------------|-----------------|  
|<span data-ttu-id="07977-108">H:\Video</span><span class="sxs-lookup"><span data-stu-id="07977-108">H:\Video</span></span>|<span data-ttu-id="07977-109">Kolekce videa, 5 TB celkem.</span><span class="sxs-lookup"><span data-stu-id="07977-109">A collection of videos, 5 TB in total.</span></span>|  
|<span data-ttu-id="07977-110">H:\Photo</span><span class="sxs-lookup"><span data-stu-id="07977-110">H:\Photo</span></span>|<span data-ttu-id="07977-111">Kolekce fotografií, celkem 30 GB.</span><span class="sxs-lookup"><span data-stu-id="07977-111">A collection of photos, 30 GB in total.</span></span>|  
|<span data-ttu-id="07977-112">K:\Temp\FavoriteMovie.ISO</span><span class="sxs-lookup"><span data-stu-id="07977-112">K:\Temp\FavoriteMovie.ISO</span></span>|<span data-ttu-id="07977-113">Image disku A Blu-ray™, 25 GB.</span><span class="sxs-lookup"><span data-stu-id="07977-113">A Blu-Ray™ disk image, 25 GB.</span></span>|  
|<span data-ttu-id="07977-114">\\\bigshare\john\music</span><span class="sxs-lookup"><span data-stu-id="07977-114">\\\bigshare\john\music</span></span>|<span data-ttu-id="07977-115">Kolekce hudebních souborů ve sdílené síťové složce, celkem 10 GB.</span><span class="sxs-lookup"><span data-stu-id="07977-115">A collection of music files on a network share, 10 GB in total.</span></span>|  
  
<span data-ttu-id="07977-116">Úloha importu Hello importuje tato data do hello následující cíle v účtu úložiště hello:</span><span class="sxs-lookup"><span data-stu-id="07977-116">hello import job imports this data into hello following destinations in hello storage account:</span></span>  
  
|<span data-ttu-id="07977-117">Zdroj</span><span class="sxs-lookup"><span data-stu-id="07977-117">Source</span></span>|<span data-ttu-id="07977-118">Cílový virtuální adresář nebo objekt blob</span><span class="sxs-lookup"><span data-stu-id="07977-118">Destination virtual directory or blob</span></span>|  
|------------|-------------------------------------------|  
|<span data-ttu-id="07977-119">H:\Video</span><span class="sxs-lookup"><span data-stu-id="07977-119">H:\Video</span></span>|<span data-ttu-id="07977-120">https://mystorageaccount.BLOB.Core.Windows.NET/video</span><span class="sxs-lookup"><span data-stu-id="07977-120">https://mystorageaccount.blob.core.windows.net/video</span></span>|  
|<span data-ttu-id="07977-121">H:\Photo</span><span class="sxs-lookup"><span data-stu-id="07977-121">H:\Photo</span></span>|<span data-ttu-id="07977-122">https://mystorageaccount.BLOB.Core.Windows.NET/Photo</span><span class="sxs-lookup"><span data-stu-id="07977-122">https://mystorageaccount.blob.core.windows.net/photo</span></span>|  
|<span data-ttu-id="07977-123">K:\Temp\FavoriteMovie.ISO</span><span class="sxs-lookup"><span data-stu-id="07977-123">K:\Temp\FavoriteMovie.ISO</span></span>|<span data-ttu-id="07977-124">https://mystorageaccount.BLOB.Core.Windows.NET/Favorite/FavoriteMovies.ISO</span><span class="sxs-lookup"><span data-stu-id="07977-124">https://mystorageaccount.blob.core.windows.net/favorite/FavoriteMovies.ISO</span></span>|  
|<span data-ttu-id="07977-125">\\\bigshare\john\music</span><span class="sxs-lookup"><span data-stu-id="07977-125">\\\bigshare\john\music</span></span>|<span data-ttu-id="07977-126">https://mystorageaccount.BLOB.Core.Windows.NET/Music</span><span class="sxs-lookup"><span data-stu-id="07977-126">https://mystorageaccount.blob.core.windows.net/music</span></span>|  
  
<span data-ttu-id="07977-127">Pomocí této mapování hello souboru `H:\Video\Drama\GreatMovie.mov` je objekt blob importované toohello `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.</span><span class="sxs-lookup"><span data-stu-id="07977-127">With this mapping, hello file `H:\Video\Drama\GreatMovie.mov` is imported toohello blob `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.</span></span>  
  
<span data-ttu-id="07977-128">Dále toodetermine kolik pevné disky jsou potřeba, výpočetní hello velikost dat hello:</span><span class="sxs-lookup"><span data-stu-id="07977-128">Next, toodetermine how many hard drives are needed, compute hello size of hello data:</span></span>  
  
`5TB + 30GB + 25GB + 10GB = 5TB + 65GB`  
  
<span data-ttu-id="07977-129">V tomto příkladu by mělo být dostatečné dva 3 TB pevné disky.</span><span class="sxs-lookup"><span data-stu-id="07977-129">For this example, two 3-TB hard drives should be sufficient.</span></span> <span data-ttu-id="07977-130">Ale protože hello zdrojový adresář `H:\Video` má 5 TB dat a jeden pevný disk je kapacita je jenom 3 TB, je nutné toobreak `H:\Video` do dvou menší adresářů: `H:\Video1` a `H:\Video2`, než spuštěna hello Microsoft Nástroj Azure importu a exportu.</span><span class="sxs-lookup"><span data-stu-id="07977-130">However, since hello source directory `H:\Video` has 5 TB of data and your single hard drive's capacity is only 3 TB, it's necessary toobreak `H:\Video` into two smaller directories: `H:\Video1` and `H:\Video2`, before running hello Microsoft Azure Import/Export Tool.</span></span> <span data-ttu-id="07977-131">Tento krok vypočítá hello následující zdrojového adresáře:</span><span class="sxs-lookup"><span data-stu-id="07977-131">This step yields hello following source directories:</span></span>  
  
|<span data-ttu-id="07977-132">Umístění</span><span class="sxs-lookup"><span data-stu-id="07977-132">Location</span></span>|<span data-ttu-id="07977-133">Velikost</span><span class="sxs-lookup"><span data-stu-id="07977-133">Size</span></span>|<span data-ttu-id="07977-134">Cílový virtuální adresář nebo objekt blob</span><span class="sxs-lookup"><span data-stu-id="07977-134">Destination virtual directory or blob</span></span>|  
|--------------|----------|-------------------------------------------|  
|<span data-ttu-id="07977-135">H:\Video1</span><span class="sxs-lookup"><span data-stu-id="07977-135">H:\Video1</span></span>|<span data-ttu-id="07977-136">2,5 TB</span><span class="sxs-lookup"><span data-stu-id="07977-136">2.5 TB</span></span>|<span data-ttu-id="07977-137">https://mystorageaccount.BLOB.Core.Windows.NET/video</span><span class="sxs-lookup"><span data-stu-id="07977-137">https://mystorageaccount.blob.core.windows.net/video</span></span>|  
|<span data-ttu-id="07977-138">H:\Video2</span><span class="sxs-lookup"><span data-stu-id="07977-138">H:\Video2</span></span>|<span data-ttu-id="07977-139">2,5 TB</span><span class="sxs-lookup"><span data-stu-id="07977-139">2.5 TB</span></span>|<span data-ttu-id="07977-140">https://mystorageaccount.BLOB.Core.Windows.NET/video</span><span class="sxs-lookup"><span data-stu-id="07977-140">https://mystorageaccount.blob.core.windows.net/video</span></span>|  
|<span data-ttu-id="07977-141">H:\Photo</span><span class="sxs-lookup"><span data-stu-id="07977-141">H:\Photo</span></span>|<span data-ttu-id="07977-142">30 GB</span><span class="sxs-lookup"><span data-stu-id="07977-142">30 GB</span></span>|<span data-ttu-id="07977-143">https://mystorageaccount.BLOB.Core.Windows.NET/Photo</span><span class="sxs-lookup"><span data-stu-id="07977-143">https://mystorageaccount.blob.core.windows.net/photo</span></span>|  
|<span data-ttu-id="07977-144">K:\Temp\FavoriteMovies.ISO</span><span class="sxs-lookup"><span data-stu-id="07977-144">K:\Temp\FavoriteMovies.ISO</span></span>|<span data-ttu-id="07977-145">25 GB</span><span class="sxs-lookup"><span data-stu-id="07977-145">25 GB</span></span>|<span data-ttu-id="07977-146">https://mystorageaccount.BLOB.Core.Windows.NET/Favorite/FavoriteMovies.ISO</span><span class="sxs-lookup"><span data-stu-id="07977-146">https://mystorageaccount.blob.core.windows.net/favorite/FavoriteMovies.ISO</span></span>|  
|<span data-ttu-id="07977-147">\\\bigshare\john\music</span><span class="sxs-lookup"><span data-stu-id="07977-147">\\\bigshare\john\music</span></span>|<span data-ttu-id="07977-148">10 GB</span><span class="sxs-lookup"><span data-stu-id="07977-148">10 GB</span></span>|<span data-ttu-id="07977-149">https://mystorageaccount.BLOB.Core.Windows.NET/Music</span><span class="sxs-lookup"><span data-stu-id="07977-149">https://mystorageaccount.blob.core.windows.net/music</span></span>|  
  
 <span data-ttu-id="07977-150">I když hello `H:\Video`directory rozdělení tootwo adresáře, budou bodu toohello stejný cílový virtuální adresář v účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="07977-150">Even though hello `H:\Video`directory has been split tootwo directories, they point toohello same destination virtual directory in hello storage account.</span></span> <span data-ttu-id="07977-151">Tímto způsobem, všechny soubory videa se udržují v rámci jedné `video` kontejneru v účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="07977-151">This way, all video files are maintained under a single `video` container in hello storage account.</span></span>  
  
 <span data-ttu-id="07977-152">V dalším kroku hello předchozí zdrojového adresáře jsou rovnoměrně distribuované toohello dva pevné disky:</span><span class="sxs-lookup"><span data-stu-id="07977-152">Next, hello previous source directories are evenly distributed toohello two hard drives:</span></span>  
  
||||  
|-|-|-|  
|<span data-ttu-id="07977-153">Pevný disk</span><span class="sxs-lookup"><span data-stu-id="07977-153">Hard drive</span></span>|<span data-ttu-id="07977-154">Zdrojové adresáře</span><span class="sxs-lookup"><span data-stu-id="07977-154">Source directories</span></span>|<span data-ttu-id="07977-155">Celková velikost</span><span class="sxs-lookup"><span data-stu-id="07977-155">Total size</span></span>|  
|<span data-ttu-id="07977-156">První jednotky</span><span class="sxs-lookup"><span data-stu-id="07977-156">First Drive</span></span>|<span data-ttu-id="07977-157">H:\Video1</span><span class="sxs-lookup"><span data-stu-id="07977-157">H:\Video1</span></span>|<span data-ttu-id="07977-158">2,5 TB + 30 GB</span><span class="sxs-lookup"><span data-stu-id="07977-158">2.5 TB + 30 GB</span></span>|  
||<span data-ttu-id="07977-159">H:\Photo</span><span class="sxs-lookup"><span data-stu-id="07977-159">H:\Photo</span></span>||  
|<span data-ttu-id="07977-160">Sekundy jednotky</span><span class="sxs-lookup"><span data-stu-id="07977-160">Second Drive</span></span>|<span data-ttu-id="07977-161">H:\Video2</span><span class="sxs-lookup"><span data-stu-id="07977-161">H:\Video2</span></span>|<span data-ttu-id="07977-162">2,5 TB + 35 GB</span><span class="sxs-lookup"><span data-stu-id="07977-162">2.5 TB + 35 GB</span></span>|  
||<span data-ttu-id="07977-163">K:\Temp\BlueRay.ISO</span><span class="sxs-lookup"><span data-stu-id="07977-163">K:\Temp\BlueRay.ISO</span></span>||  
||<span data-ttu-id="07977-164">\\\bigshare\john\music</span><span class="sxs-lookup"><span data-stu-id="07977-164">\\\bigshare\john\music</span></span>||  
  
<span data-ttu-id="07977-165">Kromě toho můžete nastavit hello následující metadata pro všechny soubory:</span><span class="sxs-lookup"><span data-stu-id="07977-165">In addition, you can set hello following metadata for all files:</span></span>  
  
-   <span data-ttu-id="07977-166">**UploadMethod:** služby Windows Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="07977-166">**UploadMethod:** Windows Azure Import/Export service</span></span>  
  
-   <span data-ttu-id="07977-167">**DataSetName:** SampleData</span><span class="sxs-lookup"><span data-stu-id="07977-167">**DataSetName:** SampleData</span></span>  
  
-   <span data-ttu-id="07977-168">**Datum vytvoření:** 10/1/2013</span><span class="sxs-lookup"><span data-stu-id="07977-168">**CreationDate:** 10/1/2013</span></span>  
  
<span data-ttu-id="07977-169">metadata tooset hello importu souborů, vytvořte textový soubor, `c:\WAImportExport\SampleMetadata.txt`, s hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="07977-169">tooset metadata for hello imported files, create a text file, `c:\WAImportExport\SampleMetadata.txt`, with hello following content:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>  
    <DataSetName>SampleData</DataSetName>  
    <CreationDate>10/1/2013</CreationDate>  
</Metadata>  
```
  
<span data-ttu-id="07977-170">Můžete vytvořit také některé vlastnosti pro hello `FavoriteMovie.ISO` objektů blob:</span><span class="sxs-lookup"><span data-stu-id="07977-170">You can also set some properties for hello `FavoriteMovie.ISO` blob:</span></span>  
  
-   <span data-ttu-id="07977-171">**Content-Type:** application/octet-stream</span><span class="sxs-lookup"><span data-stu-id="07977-171">**Content-Type:** application/octet-stream</span></span>  
  
-   <span data-ttu-id="07977-172">**Obsah MD5:** Q2hlY2sgSW50ZWdyaXR5IQ ==</span><span class="sxs-lookup"><span data-stu-id="07977-172">**Content-MD5:** Q2hlY2sgSW50ZWdyaXR5IQ==</span></span>  
  
-   <span data-ttu-id="07977-173">**Cache-Control:** no cache</span><span class="sxs-lookup"><span data-stu-id="07977-173">**Cache-Control:** no-cache</span></span>  
  
<span data-ttu-id="07977-174">tooset tyto vlastnosti, vytvořte textový soubor, `c:\WAImportExport\SampleProperties.txt`:</span><span class="sxs-lookup"><span data-stu-id="07977-174">tooset these properties, create a text file, `c:\WAImportExport\SampleProperties.txt`:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
    <Content-Type>application/octet-stream</Content-Type>  
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>  
    <Cache-Control>no-cache</Cache-Control>  
</Properties>  
```
  
<span data-ttu-id="07977-175">Teď je připraven toorun hello nástroj Azure Import/Export tooprepare hello dva pevné disky.</span><span class="sxs-lookup"><span data-stu-id="07977-175">Now you are ready toorun hello Azure Import/Export Tool tooprepare hello two hard drives.</span></span> <span data-ttu-id="07977-176">Poznámky:</span><span class="sxs-lookup"><span data-stu-id="07977-176">Note that:</span></span>  
  
-   <span data-ttu-id="07977-177">první disk Hello je připojit jako jednotka X.</span><span class="sxs-lookup"><span data-stu-id="07977-177">hello first drive is mounted as drive X.</span></span>  
  
-   <span data-ttu-id="07977-178">druhý disk Hello je připojit jako disk Y.</span><span class="sxs-lookup"><span data-stu-id="07977-178">hello second drive is mounted as drive Y.</span></span>  
  
-   <span data-ttu-id="07977-179">Hello klíč pro účet úložiště hello `mystorageaccount` je `8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg==`.</span><span class="sxs-lookup"><span data-stu-id="07977-179">hello key for hello storage account `mystorageaccount` is `8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg==`.</span></span>  

## <a name="preparing-disk-for-import-when-data-is-pre-loaded"></a><span data-ttu-id="07977-180">Příprava disku pro import, když je předem načtená data</span><span class="sxs-lookup"><span data-stu-id="07977-180">Preparing disk for import when data is pre-loaded</span></span>
 
 <span data-ttu-id="07977-181">Pokud toobe hello data importovat je již na disku hello, použijte příznak /skipwrite hello.</span><span class="sxs-lookup"><span data-stu-id="07977-181">If hello data toobe imported is already present on hello disk, use hello flag /skipwrite.</span></span> <span data-ttu-id="07977-182">Hodnota Hello /t a /srcdir má oba bodu toohello disku připraveném pro import.</span><span class="sxs-lookup"><span data-stu-id="07977-182">hello value of /t and /srcdir should both point toohello disk being prepared for import.</span></span> <span data-ttu-id="07977-183">Pokud všechny toobe hello data importovat nepůjde toohello stejné cílový virtuální adresář nebo kořenové hello účtu úložiště, spusťte hello stejný příkaz pro každý cílový adresář samostatně, udržování hello hodnota /id hello stejné napříč všechny spustí.</span><span class="sxs-lookup"><span data-stu-id="07977-183">If all of hello data toobe imported is not going toohello same destination virtual directory or root of hello storage account, run hello same command for each destination directory separately, keeping hello value of /id hello same across all runs.</span></span>

>[!NOTE] 
><span data-ttu-id="07977-184">Nezadávejte/Format, jak se bude vymazání hello dat na disku hello.</span><span class="sxs-lookup"><span data-stu-id="07977-184">Do not specify /format as it will wipe hello data on hello disk.</span></span> <span data-ttu-id="07977-185">Můžete zadat / šifrování nebo /bk v závislosti na tom, jestli hello disk už je šifrovaný nebo ne.</span><span class="sxs-lookup"><span data-stu-id="07977-185">You can specify /encrypt or /bk depending on whether hello disk is already encrypted or not.</span></span> 
>

```
    When data is already present on hello disk for each drive run hello following command.
    WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:x:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt /skipwrite
```

## <a name="copy-sessions---first-drive"></a><span data-ttu-id="07977-186">Zkopírujte relací - nejprve jednotky</span><span class="sxs-lookup"><span data-stu-id="07977-186">Copy sessions - first drive</span></span>

<span data-ttu-id="07977-187">Hello první disk spusťte hello nástroj Azure Import/Export zdroje dvakrát hello toocopy dva adresáře:</span><span class="sxs-lookup"><span data-stu-id="07977-187">For hello first drive, run hello Azure Import/Export Tool twice toocopy hello two source directories:</span></span>  

<span data-ttu-id="07977-188">**Nejdříve zkopírovat relace**</span><span class="sxs-lookup"><span data-stu-id="07977-188">**First copy session**</span></span>
  
```
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:H:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```

<span data-ttu-id="07977-189">**Druhá kopie relace**</span><span class="sxs-lookup"><span data-stu-id="07977-189">**Second copy session**</span></span>

```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Photo /srcdir:H:\Photo /dstdir:photo/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt
```

## <a name="copy-sessions---second-drive"></a><span data-ttu-id="07977-190">Zkopírujte relací - druhé jednotky</span><span class="sxs-lookup"><span data-stu-id="07977-190">Copy sessions - second drive</span></span>
 
<span data-ttu-id="07977-191">Pro hello druhý disku, spusťte hello nástroj Azure Import/Export třikrát po jednotlivých hello zdrojového adresáře a jednou pro samostatnou hello Blu-Ray™ soubor obrázku):</span><span class="sxs-lookup"><span data-stu-id="07977-191">For hello second drive, run hello Azure Import/Export Tool three times, once each for hello source directories, and once for hello standalone Blu-Ray™ image file):</span></span>  
  
<span data-ttu-id="07977-192">**Nejdříve zkopírovat relace**</span><span class="sxs-lookup"><span data-stu-id="07977-192">**First copy session**</span></span> 

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:Video2 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:y /format /encrypt /srcdir:H:\Video2 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```
  
<span data-ttu-id="07977-193">**Druhá kopie relace**</span><span class="sxs-lookup"><span data-stu-id="07977-193">**Second copy session**</span></span>

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:Music /srcdir:\\bigshare\john\music /dstdir:music/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```  
  
<span data-ttu-id="07977-194">**Třetí kopii relace**</span><span class="sxs-lookup"><span data-stu-id="07977-194">**Third copy session**</span></span>  

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:BlueRayIso /srcfile:K:\Temp\BlueRay.ISO /dstblob:favorite/BlueRay.ISO /MetadataFile:c:\WAImportExport\SampleMetadata.txt /PropertyFile:c:\WAImportExport\SampleProperties.txt  
```

## <a name="copy-session-completion"></a><span data-ttu-id="07977-195">Zkopírujte dokončení relace</span><span class="sxs-lookup"><span data-stu-id="07977-195">Copy session completion</span></span>

<span data-ttu-id="07977-196">Jakmile jste dokončili hello kopírování relací, můžete odpojit hello dvě jednotky z počítače kopie hello a dodávat je toohello příslušné služby Windows Azure datového centra.</span><span class="sxs-lookup"><span data-stu-id="07977-196">Once hello copy sessions have completed, you can disconnect hello two drives from hello copy computer and ship them toohello appropriate Windows Azure data center.</span></span> <span data-ttu-id="07977-197">Nahrát hello dva soubory deníku, `FirstDrive.jrn` a `SecondDrive.jrn`, když vytvoříte hello úlohy importu v hello [portálu Windows Azure](https://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="07977-197">Upload hello two journal files, `FirstDrive.jrn` and `SecondDrive.jrn`, when you create hello import job in hello [Windows Azure portal](https://manage.windowsazure.com/).</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="07977-198">Další kroky</span><span class="sxs-lookup"><span data-stu-id="07977-198">Next steps</span></span>

* [<span data-ttu-id="07977-199">Příprava pevných disků pro úlohu importu</span><span class="sxs-lookup"><span data-stu-id="07977-199">Preparing hard drives for an import job</span></span>](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [<span data-ttu-id="07977-200">Stručná referenční příručka pro často používané příkazy</span><span class="sxs-lookup"><span data-stu-id="07977-200">Quick reference for frequently used commands</span></span>](../storage-import-export-tool-quick-reference-v1.md) 
