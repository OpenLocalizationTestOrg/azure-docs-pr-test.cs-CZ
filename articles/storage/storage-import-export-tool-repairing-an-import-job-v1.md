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
ms.openlocfilehash: b374eabcbafa6ffe64c639fb6c89be857ecfc221
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="repairing-an-import-job"></a><span data-ttu-id="e3a5a-103">Oprava úlohy importu</span><span class="sxs-lookup"><span data-stu-id="e3a5a-103">Repairing an import job</span></span>
<span data-ttu-id="e3a5a-104">Službu Microsoft Azure Import/Export se pravděpodobně nepodaří zkopírujte některé soubory nebo části souborů do služby Windows Azure Blob.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-104">The Microsoft Azure Import/Export service may fail to copy some of your files or parts of a file to the Windows Azure Blob service.</span></span> <span data-ttu-id="e3a5a-105">Chyby z těchto důvodů:</span><span class="sxs-lookup"><span data-stu-id="e3a5a-105">Some reasons for failures include:</span></span>  
  
-   <span data-ttu-id="e3a5a-106">Poškozené soubory</span><span class="sxs-lookup"><span data-stu-id="e3a5a-106">Corrupted files</span></span>  
  
-   <span data-ttu-id="e3a5a-107">Poškozené jednotky</span><span class="sxs-lookup"><span data-stu-id="e3a5a-107">Damaged drives</span></span>  
  
-   <span data-ttu-id="e3a5a-108">Klíč účtu úložiště změnila. Přestože během přenosu souboru.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-108">The storage account key changed while the file was being transferred.</span></span>  
  
<span data-ttu-id="e3a5a-109">Můžete spustit nástroj Microsoft Azure Import/Export v importu souborů protokolu kopie úlohy a nástroj odešlete chybějící soubory (nebo části souborů) na váš účet úložiště služby Windows Azure k dokončení úlohy importu.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-109">You can run the Microsoft Azure Import/Export Tool with the import job's copy log files, and the tool will upload the missing files (or parts of a file) to your Windows Azure storage account to complete import job.</span></span>  
  
## <a name="repairimport-parameters"></a><span data-ttu-id="e3a5a-110">Parametry RepairImport</span><span class="sxs-lookup"><span data-stu-id="e3a5a-110">RepairImport parameters</span></span>

<span data-ttu-id="e3a5a-111">Mohou být zadány následující parametry s **RepairImport**:</span><span class="sxs-lookup"><span data-stu-id="e3a5a-111">The following parameters can be specified with **RepairImport**:</span></span> 
  
|||  
|-|-|  
|<span data-ttu-id="e3a5a-112">**/ r:**< RepairFile\></span><span class="sxs-lookup"><span data-stu-id="e3a5a-112">**/r:**<RepairFile\></span></span>|<span data-ttu-id="e3a5a-113">**Vyžaduje se.**</span><span class="sxs-lookup"><span data-stu-id="e3a5a-113">**Required.**</span></span> <span data-ttu-id="e3a5a-114">Cesta k souboru oprava, která sleduje průběh opravu a umožňuje obnovit přerušené opravit.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-114">Path to the repair file, which tracks the progress of the repair, and allows you to resume an interrupted repair.</span></span> <span data-ttu-id="e3a5a-115">Každé jednotky, musí mít pouze jeden soubor opravit.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-115">Each drive must have one and only one repair file.</span></span> <span data-ttu-id="e3a5a-116">Při spuštění opravy pro danou jednotku, bude předáte v cestě k oprava souboru, který ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-116">When you start a repair for a given drive, you will pass in the path to a repair file which does not yet exist.</span></span> <span data-ttu-id="e3a5a-117">Pokud chcete obnovit přerušené opravy, by měla předávat název existující soubor opravit.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-117">To resume an interrupted repair, you should pass in the name of an existing repair file.</span></span> <span data-ttu-id="e3a5a-118">Soubor opravy odpovídající cílová jednotka je vždy třeba zadat.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-118">The repair file corresponding to the target drive must always be specified.</span></span>|  
|<span data-ttu-id="e3a5a-119">**/logdir:**< LogDirectory\></span><span class="sxs-lookup"><span data-stu-id="e3a5a-119">**/logdir:**<LogDirectory\></span></span>|<span data-ttu-id="e3a5a-120">**Volitelný parametr.**</span><span class="sxs-lookup"><span data-stu-id="e3a5a-120">**Optional.**</span></span> <span data-ttu-id="e3a5a-121">K adresáři protokolů.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-121">The log directory.</span></span> <span data-ttu-id="e3a5a-122">Souborů podrobného protokolování se zapíšou do tohoto adresáře.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-122">Verbose log files will be written to this directory.</span></span> <span data-ttu-id="e3a5a-123">Pokud není zadán žádný adresář protokolu, aktuální adresář se použije jako adresář protokolu.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-123">If no log directory is specified, the current directory will be used as the log directory.</span></span>|  
|<span data-ttu-id="e3a5a-124">**/ d:**< TargetDirectories\></span><span class="sxs-lookup"><span data-stu-id="e3a5a-124">**/d:**<TargetDirectories\></span></span>|<span data-ttu-id="e3a5a-125">**Vyžaduje se.**</span><span class="sxs-lookup"><span data-stu-id="e3a5a-125">**Required.**</span></span> <span data-ttu-id="e3a5a-126">Jeden nebo více oddělených středníkem adresáře, které obsahují původní soubory, které byly naimportovány.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-126">One or more semicolon-separated directories that contain the original files that were imported.</span></span> <span data-ttu-id="e3a5a-127">Import disku mohou být využity také, ale není vyžadováno, pokud jsou k dispozici alternativní umístění původní soubory.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-127">The import drive may also be used, but is not required if alternate locations of original files are available.</span></span>|  
|<span data-ttu-id="e3a5a-128">**/BK:**< BitLockerKey\></span><span class="sxs-lookup"><span data-stu-id="e3a5a-128">**/bk:**<BitLockerKey\></span></span>|<span data-ttu-id="e3a5a-129">**Volitelný parametr.**</span><span class="sxs-lookup"><span data-stu-id="e3a5a-129">**Optional.**</span></span> <span data-ttu-id="e3a5a-130">Pokud chcete nástroj k odemknutí zašifrované jednotky kde původní soubory jsou k dispozici, je třeba zadat klíč nástroje BitLocker.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-130">You should specify the BitLocker key if you want the tool to unlock an encrypted drive where the original files are available.</span></span>|  
|<span data-ttu-id="e3a5a-131">**/sn:**< StorageAccountName\></span><span class="sxs-lookup"><span data-stu-id="e3a5a-131">**/sn:**<StorageAccountName\></span></span>|<span data-ttu-id="e3a5a-132">**Vyžaduje se.**</span><span class="sxs-lookup"><span data-stu-id="e3a5a-132">**Required.**</span></span> <span data-ttu-id="e3a5a-133">Název účtu úložiště pro úlohy importu.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-133">The name of the storage account for the import job.</span></span>|  
|<span data-ttu-id="e3a5a-134">**/Sk:**< StorageAccountKey\></span><span class="sxs-lookup"><span data-stu-id="e3a5a-134">**/sk:**<StorageAccountKey\></span></span>|<span data-ttu-id="e3a5a-135">**Požadované** Pokud není zadán sdíleného přístupového podpisu kontejneru.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-135">**Required** if and only if a container SAS is not specified.</span></span> <span data-ttu-id="e3a5a-136">Klíč účtu pro účet úložiště pro úlohy importu.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-136">The account key for the storage account for the import job.</span></span>|  
|<span data-ttu-id="e3a5a-137">**/csas:**< ContainerSas\></span><span class="sxs-lookup"><span data-stu-id="e3a5a-137">**/csas:**<ContainerSas\></span></span>|<span data-ttu-id="e3a5a-138">**Požadované** Pokud není zadaný klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-138">**Required** if and only if the storage account key is not specified.</span></span> <span data-ttu-id="e3a5a-139">Kontejner SAS pro přístup k objektům BLOB přidružený k úloze importu.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-139">The container SAS for accessing the blobs associated with the import job.</span></span>|  
|<span data-ttu-id="e3a5a-140">**/ CopyLogFile:**< DriveCopyLogFile\></span><span class="sxs-lookup"><span data-stu-id="e3a5a-140">**/CopyLogFile:**<DriveCopyLogFile\></span></span>|<span data-ttu-id="e3a5a-141">**Vyžaduje se.**</span><span class="sxs-lookup"><span data-stu-id="e3a5a-141">**Required.**</span></span> <span data-ttu-id="e3a5a-142">Cesta k souboru protokolu kopie disku (buď podrobné protokolu nebo Chyba protokolu).</span><span class="sxs-lookup"><span data-stu-id="e3a5a-142">Path to the drive copy log file (either verbose log or error log).</span></span> <span data-ttu-id="e3a5a-143">Soubor je generována pomocí služby Windows Azure Import/Export a si můžete stáhnout z úložiště objektů blob, které jsou přidružené k úloze.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-143">The file is generated by the Windows Azure Import/Export service and can be downloaded from the blob storage associated with the job.</span></span> <span data-ttu-id="e3a5a-144">Kopírovat soubor protokolu obsahuje informace o selhání objektů BLOB nebo soubory, které mají být opravit.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-144">The copy log file contains information about failed blobs or files which are to be repaired.</span></span>|  
|<span data-ttu-id="e3a5a-145">**/ PathMapFile:**< DrivePathMapFile\></span><span class="sxs-lookup"><span data-stu-id="e3a5a-145">**/PathMapFile:**<DrivePathMapFile\></span></span>|<span data-ttu-id="e3a5a-146">**Volitelný parametr.**</span><span class="sxs-lookup"><span data-stu-id="e3a5a-146">**Optional.**</span></span> <span data-ttu-id="e3a5a-147">Cesta k textový soubor, který lze vyřešit nejednoznačnosti, pokud máte více souborů se stejným názvem, který měla importu ve stejné úloze.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-147">Path to a text file that can be used to resolve ambiguities if you have multiple files with the same name that you were importing in the same job.</span></span> <span data-ttu-id="e3a5a-148">Při prvním spuštění nástroje jej můžete naplnit tento soubor se všemi nejednoznačné názvy.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-148">The first time the tool is run, it can populate this file with all of the ambiguous names.</span></span> <span data-ttu-id="e3a5a-149">Při dalším spuštění nástroje použije tento soubor přeložit nejednoznačnosti.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-149">Subsequent runs of the tool will use this file to resolve the ambiguities.</span></span>|  
  
## <a name="using-the-repairimport-command"></a><span data-ttu-id="e3a5a-150">Pomocí příkazu RepairImport</span><span class="sxs-lookup"><span data-stu-id="e3a5a-150">Using the RepairImport command</span></span>  
<span data-ttu-id="e3a5a-151">Chcete-li opravit importovat data pomocí vysílání datového proudu data přes síť, musíte zadat adresáře, které obsahují původní soubory byly import pomocí `/d` parametr.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-151">To repair import data by streaming the data over the network, you must specify the directories that contain the original files you were importing using the `/d` parameter.</span></span> <span data-ttu-id="e3a5a-152">Musíte zadat také kopírovat soubor protokolu, který jste stáhli z vašeho účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-152">You must also specify the copy log file that you downloaded from your storage account.</span></span> <span data-ttu-id="e3a5a-153">Typické příkazového řádku k opravě úlohy importu s chybami částečné vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="e3a5a-153">A typical command line to repair an import job with partial failures looks like:</span></span>  
  
```  
WAImportExport.exe RepairImport /r:C:\WAImportExport\9WM35C2V.rep /d:C:\Users\bob\Pictures;X:\BobBackup\photos /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C2V.log  
```  
  
<span data-ttu-id="e3a5a-154">Následuje příklad kopírování souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-154">The following is an example of a copy log file.</span></span> <span data-ttu-id="e3a5a-155">V tomto případě jednoho 64 tisíc část soubor byl poškozen na jednotce, která byla odeslaná úlohy importu.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-155">In this case, one 64K piece of a file was corrupted on the drive that was shipped for the import job.</span></span> <span data-ttu-id="e3a5a-156">Protože se jedná pouze selhání uvedené, zbytek objektů BLOB v úlohy byly úspěšně importovány.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-156">Since this is the only failure indicated, the rest of the blobs in the job were successfully imported.</span></span>  
  
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
  
<span data-ttu-id="e3a5a-157">Když tento protokol kopie předána nástroj Azure Import/Export, tento nástroj se pokusí dokončit import tohoto souboru kopírování chybějící obsahu přes síť.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-157">When this copy log is passed to the Azure Import/Export Tool, the tool will try to finish the import for this file by copying the missing contents across the network.</span></span> <span data-ttu-id="e3a5a-158">Po výše uvedeném příkladu bude nástroj Hledat původní soubor `\animals\koala.jpg` v rámci dva adresáře `C:\Users\bob\Pictures` a `X:\BobBackup\photos`.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-158">Following the example above, the tool will look for the original file `\animals\koala.jpg` within the two directories `C:\Users\bob\Pictures` and `X:\BobBackup\photos`.</span></span> <span data-ttu-id="e3a5a-159">Pokud soubor `C:\Users\bob\Pictures\animals\koala.jpg` existuje, nástroj Azure Import/Export zkopíruje chybějící oblasti dat na odpovídající objekt blob `http://bobmediaaccount.blob.core.windows.net/pictures/animals/koala.jpg`.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-159">If the file `C:\Users\bob\Pictures\animals\koala.jpg` exists, the Azure Import/Export Tool will copy the missing range of data to the corresponding blob `http://bobmediaaccount.blob.core.windows.net/pictures/animals/koala.jpg`.</span></span>  
  
## <a name="resolving-conflicts-when-using-repairimport"></a><span data-ttu-id="e3a5a-160">Řešení konfliktů při použití RepairImport</span><span class="sxs-lookup"><span data-stu-id="e3a5a-160">Resolving conflicts when using RepairImport</span></span>  
<span data-ttu-id="e3a5a-161">V některých situacích nástroj pravděpodobně nebude moci najít nebo otevřít soubor potřebné pro jednu z následujících důvodů: soubor nebyl nalezen, soubor není dostupný, je nejednoznačný název souboru nebo obsah souboru již není správný.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-161">In some situations, the tool may not be able to find or open the necessary file for one of the following reasons: the file could not be found, the file is not accessible, the file name is ambiguous, or the content of the file is no longer correct.</span></span>  
  
<span data-ttu-id="e3a5a-162">Nejednoznačné chybě může dojít, pokud tento nástroj se pokouší vyhledat `\animals\koala.jpg` a existuje soubor s tímto názvem v obou `C:\Users\bob\pictures` a `X:\BobBackup\photos`.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-162">An ambiguous error could occur if the tool is trying to locate `\animals\koala.jpg` and there is a file with that name under both `C:\Users\bob\pictures` and `X:\BobBackup\photos`.</span></span> <span data-ttu-id="e3a5a-163">To znamená i `C:\Users\bob\pictures\animals\koala.jpg` a `X:\BobBackup\photos\animals\koala.jpg` existovat na jednotkách úlohy importu.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-163">That is, both `C:\Users\bob\pictures\animals\koala.jpg` and `X:\BobBackup\photos\animals\koala.jpg` exist on the import job drives.</span></span>  
  
<span data-ttu-id="e3a5a-164">`/PathMapFile` Možnost vám umožňuje vyřešte tyto chyby.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-164">The `/PathMapFile` option will allow you to resolve these errors.</span></span> <span data-ttu-id="e3a5a-165">Můžete zadat název souboru, který bude obsahuje seznam souborů, které tento nástroj se nepodařilo správně identifikovat.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-165">You can specify the name of the file which will contains the list of files that the tool was not able to correctly identify.</span></span> <span data-ttu-id="e3a5a-166">Tady je příklad příkazového řádku, na které by naplňuje `9WM35C2V_pathmap.txt`:</span><span class="sxs-lookup"><span data-stu-id="e3a5a-166">The following is an example command line that would populate `9WM35C2V_pathmap.txt`:</span></span>  
  
```
WAImportExport.exe RepairImport /r:C:\WAImportExport\9WM35C2V.rep /d:C:\Users\bob\Pictures;X:\BobBackup\photos /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C2V.log /PathMapFile:C:\WAImportExport\9WM35C2V_pathmap.txt  
```
  
<span data-ttu-id="e3a5a-167">Tento nástroj pak zapíše problematické cesty k `9WM35C2V_pathmap.txt`, každou na jeden řádek.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-167">The tool will then write the problematic file paths to `9WM35C2V_pathmap.txt`, one on each line.</span></span> <span data-ttu-id="e3a5a-168">Například soubor může obsahovat následující položky po spuštění příkazu:</span><span class="sxs-lookup"><span data-stu-id="e3a5a-168">For instance, the file may contain the following entries after running the command:</span></span>  
 
```
\animals\koala.jpg  
\animals\kangaroo.jpg  
```
  
 <span data-ttu-id="e3a5a-169">Pro každý soubor v seznamu by měl pokusí najít a otevřít soubor zkontrolujte, zda že je k dispozici pro nástroj.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-169">For each file in the list, you should attempt to locate and open the file to ensure it is available to the tool.</span></span> <span data-ttu-id="e3a5a-170">Pokud chcete zjistit nástroj explicitně, kde najít soubor, můžete upravit cestě k souboru mapy a přidejte cestu ke každému souboru na stejném řádku, oddělených tabulátorem:</span><span class="sxs-lookup"><span data-stu-id="e3a5a-170">If you wish to tell the tool explicitly where to find a file, you can modify the path map file and add the path to each file on the same line, separated by a tab character:</span></span>  
  
```
\animals\koala.jpg           C:\Users\bob\Pictures\animals\koala.jpg  
\animals\kangaroo.jpg        X:\BobBackup\photos\animals\kangaroo.jpg  
```
  
<span data-ttu-id="e3a5a-171">Po zpřístupnění nástroje potřebné soubory nebo aktualizaci souboru s mapováním cestu, můžete znovu spustit nástroj pro dokončení procesu importu.</span><span class="sxs-lookup"><span data-stu-id="e3a5a-171">After making the necessary files available to the tool, or updating the path map file, you can rerun the tool to complete the import process.</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="e3a5a-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e3a5a-172">Next steps</span></span>
 
* [<span data-ttu-id="e3a5a-173">Nastavení nástroje Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="e3a5a-173">Setting Up the Azure Import/Export Tool</span></span>](storage-import-export-tool-setup-v1.md)   
* [<span data-ttu-id="e3a5a-174">Příprava pevných disků pro úlohu importu</span><span class="sxs-lookup"><span data-stu-id="e3a5a-174">Preparing hard drives for an import job</span></span>](storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [<span data-ttu-id="e3a5a-175">Kontrola stavu úlohy s použitím kopií souborů protokolu</span><span class="sxs-lookup"><span data-stu-id="e3a5a-175">Reviewing job status with copy log files</span></span>](storage-import-export-tool-reviewing-job-status-v1.md)   
* [<span data-ttu-id="e3a5a-176">Oprava úlohy exportu</span><span class="sxs-lookup"><span data-stu-id="e3a5a-176">Repairing an export job</span></span>](storage-import-export-tool-repairing-an-export-job-v1.md)   
* [<span data-ttu-id="e3a5a-177">Řešení potíží s nástrojem Azure pro import/export</span><span class="sxs-lookup"><span data-stu-id="e3a5a-177">Troubleshooting the Azure Import/Export Tool</span></span>](storage-import-export-tool-troubleshooting-v1.md)
