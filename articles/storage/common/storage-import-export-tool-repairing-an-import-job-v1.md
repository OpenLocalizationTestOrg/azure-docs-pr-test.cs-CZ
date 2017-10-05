---
title: "Oprava úlohy importu Azure Import/Export - v1 | Microsoft Docs"
description: "Zjistěte, jak opravit úlohu importu, který byl vytvořen a spuštění pomocí služby Azure Import/Export."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 38cc16bd-ad55-4625-9a85-e1726c35fd1b
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: c837713fd9e7d03287ae5a3644fd6bb47714c9d4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="repairing-an-import-job"></a><span data-ttu-id="f5963-103">Oprava úlohy importu</span><span class="sxs-lookup"><span data-stu-id="f5963-103">Repairing an import job</span></span>
<span data-ttu-id="f5963-104">Službu Microsoft Azure Import/Export se pravděpodobně nepodaří zkopírujte některé soubory nebo části souborů do služby Windows Azure Blob.</span><span class="sxs-lookup"><span data-stu-id="f5963-104">The Microsoft Azure Import/Export service may fail to copy some of your files or parts of a file to the Windows Azure Blob service.</span></span> <span data-ttu-id="f5963-105">Chyby z těchto důvodů:</span><span class="sxs-lookup"><span data-stu-id="f5963-105">Some reasons for failures include:</span></span>  
  
-   <span data-ttu-id="f5963-106">Poškozené soubory</span><span class="sxs-lookup"><span data-stu-id="f5963-106">Corrupted files</span></span>  
  
-   <span data-ttu-id="f5963-107">Poškozené jednotky</span><span class="sxs-lookup"><span data-stu-id="f5963-107">Damaged drives</span></span>  
  
-   <span data-ttu-id="f5963-108">Klíč účtu úložiště změnila. Přestože během přenosu souboru.</span><span class="sxs-lookup"><span data-stu-id="f5963-108">The storage account key changed while the file was being transferred.</span></span>  
  
<span data-ttu-id="f5963-109">Můžete spustit nástroj Microsoft Azure Import/Export v importu souborů protokolu kopie úlohy a nástroj odesílá chybějící soubory (nebo části souborů) na váš účet úložiště služby Windows Azure k dokončení úlohy importu.</span><span class="sxs-lookup"><span data-stu-id="f5963-109">You can run the Microsoft Azure Import/Export Tool with the import job's copy log files, and the tool uploads the missing files (or parts of a file) to your Windows Azure storage account to complete the import job.</span></span>  
  
## <a name="repairimport-parameters"></a><span data-ttu-id="f5963-110">Parametry RepairImport</span><span class="sxs-lookup"><span data-stu-id="f5963-110">RepairImport parameters</span></span>

<span data-ttu-id="f5963-111">Mohou být zadány následující parametry s **RepairImport**:</span><span class="sxs-lookup"><span data-stu-id="f5963-111">The following parameters can be specified with **RepairImport**:</span></span> 
  
|||  
|-|-|  
|<span data-ttu-id="f5963-112">**/ r:**< RepairFile\></span><span class="sxs-lookup"><span data-stu-id="f5963-112">**/r:**<RepairFile\></span></span>|<span data-ttu-id="f5963-113">**Vyžaduje se.**</span><span class="sxs-lookup"><span data-stu-id="f5963-113">**Required.**</span></span> <span data-ttu-id="f5963-114">Cesta k souboru oprava, která sleduje průběh opravu a umožňuje obnovit přerušené opravit.</span><span class="sxs-lookup"><span data-stu-id="f5963-114">Path to the repair file, which tracks the progress of the repair, and allows you to resume an interrupted repair.</span></span> <span data-ttu-id="f5963-115">Každé jednotky, musí mít pouze jeden soubor opravit.</span><span class="sxs-lookup"><span data-stu-id="f5963-115">Each drive must have one and only one repair file.</span></span> <span data-ttu-id="f5963-116">Když spustíte opravy pro danou jednotku, předejte v cestě oprava souboru, který ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="f5963-116">When you start a repair for a given drive, pass in the path to a repair file, which does not yet exist.</span></span> <span data-ttu-id="f5963-117">Pokud chcete obnovit přerušené opravy, by měla předávat název existující soubor opravit.</span><span class="sxs-lookup"><span data-stu-id="f5963-117">To resume an interrupted repair, you should pass in the name of an existing repair file.</span></span> <span data-ttu-id="f5963-118">Soubor opravy odpovídající cílová jednotka je vždy třeba zadat.</span><span class="sxs-lookup"><span data-stu-id="f5963-118">The repair file corresponding to the target drive must always be specified.</span></span>|  
|<span data-ttu-id="f5963-119">**/logdir:**< LogDirectory\></span><span class="sxs-lookup"><span data-stu-id="f5963-119">**/logdir:**<LogDirectory\></span></span>|<span data-ttu-id="f5963-120">**Volitelný parametr.**</span><span class="sxs-lookup"><span data-stu-id="f5963-120">**Optional.**</span></span> <span data-ttu-id="f5963-121">K adresáři protokolů.</span><span class="sxs-lookup"><span data-stu-id="f5963-121">The log directory.</span></span> <span data-ttu-id="f5963-122">Podrobné soubory protokolu se zapisují do tohoto adresáře.</span><span class="sxs-lookup"><span data-stu-id="f5963-122">Verbose log files are written to this directory.</span></span> <span data-ttu-id="f5963-123">Pokud není zadán žádný adresář protokolu, aktuální adresář se používá jako adresář protokolu.</span><span class="sxs-lookup"><span data-stu-id="f5963-123">If no log directory is specified, the current directory is used as the log directory.</span></span>|  
|<span data-ttu-id="f5963-124">**/ d:**< TargetDirectories\></span><span class="sxs-lookup"><span data-stu-id="f5963-124">**/d:**<TargetDirectories\></span></span>|<span data-ttu-id="f5963-125">**Vyžaduje se.**</span><span class="sxs-lookup"><span data-stu-id="f5963-125">**Required.**</span></span> <span data-ttu-id="f5963-126">Jeden nebo více oddělených středníkem adresáře, které obsahují původní soubory, které byly naimportovány.</span><span class="sxs-lookup"><span data-stu-id="f5963-126">One or more semicolon-separated directories that contain the original files that were imported.</span></span> <span data-ttu-id="f5963-127">Import disku mohou být využity také, ale není vyžadováno, pokud jsou k dispozici alternativní umístění původní soubory.</span><span class="sxs-lookup"><span data-stu-id="f5963-127">The import drive may also be used, but is not required if alternate locations of original files are available.</span></span>|  
|<span data-ttu-id="f5963-128">**/BK:**< BitLockerKey\></span><span class="sxs-lookup"><span data-stu-id="f5963-128">**/bk:**<BitLockerKey\></span></span>|<span data-ttu-id="f5963-129">**Volitelný parametr.**</span><span class="sxs-lookup"><span data-stu-id="f5963-129">**Optional.**</span></span> <span data-ttu-id="f5963-130">Pokud chcete nástroj k odemknutí zašifrované jednotky kde původní soubory jsou k dispozici, je třeba zadat klíč nástroje BitLocker.</span><span class="sxs-lookup"><span data-stu-id="f5963-130">You should specify the BitLocker key if you want the tool to unlock an encrypted drive where the original files are available.</span></span>|  
|<span data-ttu-id="f5963-131">**/sn:**< StorageAccountName\></span><span class="sxs-lookup"><span data-stu-id="f5963-131">**/sn:**<StorageAccountName\></span></span>|<span data-ttu-id="f5963-132">**Vyžaduje se.**</span><span class="sxs-lookup"><span data-stu-id="f5963-132">**Required.**</span></span> <span data-ttu-id="f5963-133">Název účtu úložiště pro úlohy importu.</span><span class="sxs-lookup"><span data-stu-id="f5963-133">The name of the storage account for the import job.</span></span>|  
|<span data-ttu-id="f5963-134">**/Sk:**< StorageAccountKey\></span><span class="sxs-lookup"><span data-stu-id="f5963-134">**/sk:**<StorageAccountKey\></span></span>|<span data-ttu-id="f5963-135">**Požadované** Pokud není zadán sdíleného přístupového podpisu kontejneru.</span><span class="sxs-lookup"><span data-stu-id="f5963-135">**Required** if and only if a container SAS is not specified.</span></span> <span data-ttu-id="f5963-136">Klíč účtu pro účet úložiště pro úlohy importu.</span><span class="sxs-lookup"><span data-stu-id="f5963-136">The account key for the storage account for the import job.</span></span>|  
|<span data-ttu-id="f5963-137">**/csas:**< ContainerSas\></span><span class="sxs-lookup"><span data-stu-id="f5963-137">**/csas:**<ContainerSas\></span></span>|<span data-ttu-id="f5963-138">**Požadované** Pokud není zadaný klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="f5963-138">**Required** if and only if the storage account key is not specified.</span></span> <span data-ttu-id="f5963-139">Kontejner SAS pro přístup k objektům BLOB přidružený k úloze importu.</span><span class="sxs-lookup"><span data-stu-id="f5963-139">The container SAS for accessing the blobs associated with the import job.</span></span>|  
|<span data-ttu-id="f5963-140">**/ CopyLogFile:**< DriveCopyLogFile\></span><span class="sxs-lookup"><span data-stu-id="f5963-140">**/CopyLogFile:**<DriveCopyLogFile\></span></span>|<span data-ttu-id="f5963-141">**Vyžaduje se.**</span><span class="sxs-lookup"><span data-stu-id="f5963-141">**Required.**</span></span> <span data-ttu-id="f5963-142">Cesta k souboru protokolu kopie disku (buď podrobné protokolu nebo Chyba protokolu).</span><span class="sxs-lookup"><span data-stu-id="f5963-142">Path to the drive copy log file (either verbose log or error log).</span></span> <span data-ttu-id="f5963-143">Soubor je generována pomocí služby Windows Azure Import/Export a si můžete stáhnout z úložiště objektů blob, které jsou přidružené k úloze.</span><span class="sxs-lookup"><span data-stu-id="f5963-143">The file is generated by the Windows Azure Import/Export service and can be downloaded from the blob storage associated with the job.</span></span> <span data-ttu-id="f5963-144">Kopírovat soubor protokolu obsahuje informace o selhání objekty BLOB nebo soubory, které mají být opravit.</span><span class="sxs-lookup"><span data-stu-id="f5963-144">The copy log file contains information about failed blobs or files, which are to be repaired.</span></span>|  
|<span data-ttu-id="f5963-145">**/ PathMapFile:**< DrivePathMapFile\></span><span class="sxs-lookup"><span data-stu-id="f5963-145">**/PathMapFile:**<DrivePathMapFile\></span></span>|<span data-ttu-id="f5963-146">**Volitelný parametr.**</span><span class="sxs-lookup"><span data-stu-id="f5963-146">**Optional.**</span></span> <span data-ttu-id="f5963-147">Cesta k textový soubor, který lze vyřešit nejednoznačnosti, pokud máte více souborů se stejným názvem, který měla importu ve stejné úloze.</span><span class="sxs-lookup"><span data-stu-id="f5963-147">Path to a text file that can be used to resolve ambiguities if you have multiple files with the same name that you were importing in the same job.</span></span> <span data-ttu-id="f5963-148">Při prvním spuštění nástroje jej můžete naplnit tento soubor se všemi nejednoznačné názvy.</span><span class="sxs-lookup"><span data-stu-id="f5963-148">The first time the tool is run, it can populate this file with all of the ambiguous names.</span></span> <span data-ttu-id="f5963-149">Při dalším spuštění nástroje pro tento soubor použít přeložit nejednoznačnosti.</span><span class="sxs-lookup"><span data-stu-id="f5963-149">Subsequent runs of the tool use this file to resolve the ambiguities.</span></span>|  
  
## <a name="using-the-repairimport-command"></a><span data-ttu-id="f5963-150">Pomocí příkazu RepairImport</span><span class="sxs-lookup"><span data-stu-id="f5963-150">Using the RepairImport command</span></span>  
<span data-ttu-id="f5963-151">Chcete-li opravit importovat data pomocí vysílání datového proudu data přes síť, musíte zadat adresáře, které obsahují původní soubory byly import pomocí `/d` parametr.</span><span class="sxs-lookup"><span data-stu-id="f5963-151">To repair import data by streaming the data over the network, you must specify the directories that contain the original files you were importing using the `/d` parameter.</span></span> <span data-ttu-id="f5963-152">Musíte zadat také kopírovat soubor protokolu, který jste stáhli z vašeho účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="f5963-152">You must also specify the copy log file that you downloaded from your storage account.</span></span> <span data-ttu-id="f5963-153">Typické příkazového řádku k opravě úlohy importu s chybami částečné vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="f5963-153">A typical command line to repair an import job with partial failures looks like:</span></span>  
  
```  
WAImportExport.exe RepairImport /r:C:\WAImportExport\9WM35C2V.rep /d:C:\Users\bob\Pictures;X:\BobBackup\photos /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C2V.log  
```  
  
<span data-ttu-id="f5963-154">V následujícím příkladu kopírování souboru protokolu byl poškozen jeden 64 tisíc část souboru na jednotce, která byla odeslaná úlohy importu.</span><span class="sxs-lookup"><span data-stu-id="f5963-154">In the following example of a copy log file, one 64-K piece of a file was corrupted on the drive that was shipped for the import job.</span></span> <span data-ttu-id="f5963-155">Protože se jedná pouze selhání uvedené, zbytek objektů BLOB v úlohy byly úspěšně importovány.</span><span class="sxs-lookup"><span data-stu-id="f5963-155">Since this is the only failure indicated, the rest of the blobs in the job were successfully imported.</span></span>  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveLog>  
 <DriveId>9WM35C2V</DriveId>  
 <Blob Status="CompletedWithErrors">  
 <BlobPath>pictures/animals/koala.jpg</BlobPath>  
 <FilePath>\animals\koala.jpg</FilePath>  
 <Length>163840</Length>  
 <ImportDisposition Status="Overwritten">overwrite</ImportDisposition>  
 <PageRangeList>  
  <PageRange Offset="65536" Length="65536" Hash="AA2585F6F6FD01C4AD4256E018240CD4" Status="Corrupted" />  
 </PageRangeList>  
 </Blob>  
 <Status>CompletedWithErrors</Status>  
</DriveLog>  
```
  
<span data-ttu-id="f5963-156">Když tento protokol kopie předána nástroj Azure Import/Export, pokusí se dokončit import tohoto souboru kopírování chybějící obsahu přes síť.</span><span class="sxs-lookup"><span data-stu-id="f5963-156">When this copy log is passed to the Azure Import/Export Tool, the tool tries to finish the import for this file by copying the missing contents across the network.</span></span> <span data-ttu-id="f5963-157">Po výše uvedeném příkladu nástroj vyhledá původní soubor `\animals\koala.jpg` v rámci dva adresáře `C:\Users\bob\Pictures` a `X:\BobBackup\photos`.</span><span class="sxs-lookup"><span data-stu-id="f5963-157">Following the example above, the tool looks for the original file `\animals\koala.jpg` within the two directories `C:\Users\bob\Pictures` and `X:\BobBackup\photos`.</span></span> <span data-ttu-id="f5963-158">Pokud soubor `C:\Users\bob\Pictures\animals\koala.jpg` existuje, nástroj Azure Import/Export zkopíruje chybějící oblasti dat na odpovídající objekt blob `http://bobmediaaccount.blob.core.windows.net/pictures/animals/koala.jpg`.</span><span class="sxs-lookup"><span data-stu-id="f5963-158">If the file `C:\Users\bob\Pictures\animals\koala.jpg` exists, the Azure Import/Export Tool copies the missing range of data to the corresponding blob `http://bobmediaaccount.blob.core.windows.net/pictures/animals/koala.jpg`.</span></span>  
  
## <a name="resolving-conflicts-when-using-repairimport"></a><span data-ttu-id="f5963-159">Řešení konfliktů při použití RepairImport</span><span class="sxs-lookup"><span data-stu-id="f5963-159">Resolving conflicts when using RepairImport</span></span>  
<span data-ttu-id="f5963-160">V některých situacích nástroj pravděpodobně nebude moci najít nebo otevřít soubor potřebné pro jednu z následujících důvodů: soubor nebyl nalezen, soubor není dostupný, je nejednoznačný název souboru nebo obsah souboru již není správný.</span><span class="sxs-lookup"><span data-stu-id="f5963-160">In some situations, the tool may not be able to find or open the necessary file for one of the following reasons: the file could not be found, the file is not accessible, the file name is ambiguous, or the content of the file is no longer correct.</span></span>  
  
<span data-ttu-id="f5963-161">Nejednoznačné chybě může dojít, pokud tento nástroj se pokouší vyhledat `\animals\koala.jpg` a existuje soubor s tímto názvem v obou `C:\Users\bob\pictures` a `X:\BobBackup\photos`.</span><span class="sxs-lookup"><span data-stu-id="f5963-161">An ambiguous error could occur if the tool is trying to locate `\animals\koala.jpg` and there is a file with that name under both `C:\Users\bob\pictures` and `X:\BobBackup\photos`.</span></span> <span data-ttu-id="f5963-162">To znamená i `C:\Users\bob\pictures\animals\koala.jpg` a `X:\BobBackup\photos\animals\koala.jpg` existovat na jednotkách úlohy importu.</span><span class="sxs-lookup"><span data-stu-id="f5963-162">That is, both `C:\Users\bob\pictures\animals\koala.jpg` and `X:\BobBackup\photos\animals\koala.jpg` exist on the import job drives.</span></span>  
  
<span data-ttu-id="f5963-163">`/PathMapFile` Možnost umožňuje vyřešte tyto chyby.</span><span class="sxs-lookup"><span data-stu-id="f5963-163">The `/PathMapFile` option allows you to resolve these errors.</span></span> <span data-ttu-id="f5963-164">Můžete zadat název souboru, který obsahuje seznam souborů, které tento nástroj se nepodařilo správně identifikovat.</span><span class="sxs-lookup"><span data-stu-id="f5963-164">You can specify the name of the file, which contains the list of files that the tool was not able to correctly identify.</span></span> <span data-ttu-id="f5963-165">V následujícím příkladu příkazového řádku naplní `9WM35C2V_pathmap.txt`:</span><span class="sxs-lookup"><span data-stu-id="f5963-165">The following command-line example populates `9WM35C2V_pathmap.txt`:</span></span>  
  
```
WAImportExport.exe RepairImport /r:C:\WAImportExport\9WM35C2V.rep /d:C:\Users\bob\Pictures;X:\BobBackup\photos /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C2V.log /PathMapFile:C:\WAImportExport\9WM35C2V_pathmap.txt  
```
  
<span data-ttu-id="f5963-166">Tento nástroj pak zapíše problematické cesty k `9WM35C2V_pathmap.txt`, každou na jeden řádek.</span><span class="sxs-lookup"><span data-stu-id="f5963-166">The tool will then write the problematic file paths to `9WM35C2V_pathmap.txt`, one on each line.</span></span> <span data-ttu-id="f5963-167">Například soubor může obsahovat následující položky po spuštění příkazu:</span><span class="sxs-lookup"><span data-stu-id="f5963-167">For instance, the file may contain the following entries after running the command:</span></span>  
 
```
\animals\koala.jpg  
\animals\kangaroo.jpg  
```
  
 <span data-ttu-id="f5963-168">Pro každý soubor v seznamu by měl pokusí najít a otevřít soubor zkontrolujte, zda že je k dispozici pro nástroj.</span><span class="sxs-lookup"><span data-stu-id="f5963-168">For each file in the list, you should attempt to locate and open the file to ensure it is available to the tool.</span></span> <span data-ttu-id="f5963-169">Pokud chcete zjistit nástroj explicitně, kde najít soubor, můžete upravit cestě k souboru mapy a přidejte cestu ke každému souboru na stejném řádku, oddělených tabulátorem:</span><span class="sxs-lookup"><span data-stu-id="f5963-169">If you wish to tell the tool explicitly where to find a file, you can modify the path map file and add the path to each file on the same line, separated by a tab character:</span></span>  
  
```
\animals\koala.jpg           C:\Users\bob\Pictures\animals\koala.jpg  
\animals\kangaroo.jpg        X:\BobBackup\photos\animals\kangaroo.jpg  
```
  
<span data-ttu-id="f5963-170">Po zpřístupnění nástroje potřebné soubory nebo aktualizaci souboru s mapováním cestu, můžete znovu spustit nástroj pro dokončení procesu importu.</span><span class="sxs-lookup"><span data-stu-id="f5963-170">After making the necessary files available to the tool, or updating the path map file, you can rerun the tool to complete the import process.</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="f5963-171">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f5963-171">Next steps</span></span>
 
* [<span data-ttu-id="f5963-172">Nastavení nástroje Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="f5963-172">Setting Up the Azure Import/Export Tool</span></span>](storage-import-export-tool-setup-v1.md)   
* [<span data-ttu-id="f5963-173">Příprava pevných disků pro úlohu importu</span><span class="sxs-lookup"><span data-stu-id="f5963-173">Preparing hard drives for an import job</span></span>](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [<span data-ttu-id="f5963-174">Kontrola stavu úlohy s použitím kopií souborů protokolu</span><span class="sxs-lookup"><span data-stu-id="f5963-174">Reviewing job status with copy log files</span></span>](storage-import-export-tool-reviewing-job-status-v1.md)   
* [<span data-ttu-id="f5963-175">Oprava úlohy exportu</span><span class="sxs-lookup"><span data-stu-id="f5963-175">Repairing an export job</span></span>](../storage-import-export-tool-repairing-an-export-job-v1.md)   
* [<span data-ttu-id="f5963-176">Řešení potíží s nástrojem Azure pro import/export</span><span class="sxs-lookup"><span data-stu-id="f5963-176">Troubleshooting the Azure Import/Export Tool</span></span>](storage-import-export-tool-troubleshooting-v1.md)
