---
title: "Oprava úlohy exportu Azure Import/Export - v1 | Microsoft Docs"
description: "Zjistěte, jak opravit úlohu export, který byl vytvořen a spuštění pomocí služby Azure Import/Export."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 728e2a42-04ce-4be8-9375-e9e2bc6827a5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 30ca0f8d06cb1927c19e66035ff485db0fc09e5a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="repairing-an-export-job"></a><span data-ttu-id="a45fc-103">Oprava úlohy exportu</span><span class="sxs-lookup"><span data-stu-id="a45fc-103">Repairing an export job</span></span>
<span data-ttu-id="a45fc-104">Po dokončení úlohy exportu, můžete spustit nástroj Microsoft Azure Import/Export místní na:</span><span class="sxs-lookup"><span data-stu-id="a45fc-104">After an export job has completed, you can run the Microsoft Azure Import/Export Tool on-premises to:</span></span>  
  
1.  <span data-ttu-id="a45fc-105">Stáhněte všechny soubory, které služba Azure Import/Export se nepodařilo exportovat.</span><span class="sxs-lookup"><span data-stu-id="a45fc-105">Download any files that the Azure Import/Export service was unable to export.</span></span>  
  
2.  <span data-ttu-id="a45fc-106">Ověření, aby byly správně exportovány soubory na disku.</span><span class="sxs-lookup"><span data-stu-id="a45fc-106">Validate that the files on the drive were correctly exported.</span></span>  
  
<span data-ttu-id="a45fc-107">Musí mít připojení k Azure Storage k použití této funkce.</span><span class="sxs-lookup"><span data-stu-id="a45fc-107">You must have connectivity to Azure Storage to use this functionality.</span></span>  
  
<span data-ttu-id="a45fc-108">Příkaz pro opravu úlohy importu je **RepairExport**.</span><span class="sxs-lookup"><span data-stu-id="a45fc-108">The command for repairing an import job is **RepairExport**.</span></span>

## <a name="repairexport-parameters"></a><span data-ttu-id="a45fc-109">Parametry RepairExport</span><span class="sxs-lookup"><span data-stu-id="a45fc-109">RepairExport parameters</span></span>

<span data-ttu-id="a45fc-110">Mohou být zadány následující parametry s **RepairExport**:</span><span class="sxs-lookup"><span data-stu-id="a45fc-110">The following parameters can be specified with **RepairExport**:</span></span>  
  
|<span data-ttu-id="a45fc-111">Parametr</span><span class="sxs-lookup"><span data-stu-id="a45fc-111">Parameter</span></span>|<span data-ttu-id="a45fc-112">Popis</span><span class="sxs-lookup"><span data-stu-id="a45fc-112">Description</span></span>|  
|---------------|-----------------|  
|<span data-ttu-id="a45fc-113">**/ r: < RepairFile\>**</span><span class="sxs-lookup"><span data-stu-id="a45fc-113">**/r:<RepairFile\>**</span></span>|<span data-ttu-id="a45fc-114">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="a45fc-114">Required.</span></span> <span data-ttu-id="a45fc-115">Cesta k souboru oprava, která sleduje průběh opravu a umožňuje obnovit přerušené opravit.</span><span class="sxs-lookup"><span data-stu-id="a45fc-115">Path to the repair file, which tracks the progress of the repair, and allows you to resume an interrupted repair.</span></span> <span data-ttu-id="a45fc-116">Každé jednotky, musí mít pouze jeden soubor opravit.</span><span class="sxs-lookup"><span data-stu-id="a45fc-116">Each drive must have one and only one repair file.</span></span> <span data-ttu-id="a45fc-117">Při spuštění opravy pro danou jednotku, bude předáte v cestě k oprava souboru, který ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="a45fc-117">When you start a repair for a given drive, you will pass in the path to a repair file which does not yet exist.</span></span> <span data-ttu-id="a45fc-118">Pokud chcete obnovit přerušené opravy, by měla předávat název existující soubor opravit.</span><span class="sxs-lookup"><span data-stu-id="a45fc-118">To resume an interrupted repair, you should pass in the name of an existing repair file.</span></span> <span data-ttu-id="a45fc-119">Soubor opravy odpovídající cílová jednotka je vždy třeba zadat.</span><span class="sxs-lookup"><span data-stu-id="a45fc-119">The repair file corresponding to the target drive must always be specified.</span></span>|  
|<span data-ttu-id="a45fc-120">**/logdir: < LogDirectory\>**</span><span class="sxs-lookup"><span data-stu-id="a45fc-120">**/logdir:<LogDirectory\>**</span></span>|<span data-ttu-id="a45fc-121">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="a45fc-121">Optional.</span></span> <span data-ttu-id="a45fc-122">K adresáři protokolů.</span><span class="sxs-lookup"><span data-stu-id="a45fc-122">The log directory.</span></span> <span data-ttu-id="a45fc-123">Souborů podrobného protokolování se zapíšou do tohoto adresáře.</span><span class="sxs-lookup"><span data-stu-id="a45fc-123">Verbose log files will be written to this directory.</span></span> <span data-ttu-id="a45fc-124">Pokud není zadán žádný adresář protokolu, aktuální adresář se použije jako adresář protokolu.</span><span class="sxs-lookup"><span data-stu-id="a45fc-124">If no log directory is specified, the current directory will be used as the log directory.</span></span>|  
|<span data-ttu-id="a45fc-125">**/ d: < TargetDirectory\>**</span><span class="sxs-lookup"><span data-stu-id="a45fc-125">**/d:<TargetDirectory\>**</span></span>|<span data-ttu-id="a45fc-126">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="a45fc-126">Required.</span></span> <span data-ttu-id="a45fc-127">Adresář, který chcete ověřit a opravit.</span><span class="sxs-lookup"><span data-stu-id="a45fc-127">The directory to validate and repair.</span></span> <span data-ttu-id="a45fc-128">To je obvykle kořenový adresář jednotky exportu, ale může také být do síťové sdílené složky obsahující kopii exportované soubory.</span><span class="sxs-lookup"><span data-stu-id="a45fc-128">This is usually the root directory of the export drive, but could also be a network file share containing a copy of the exported files.</span></span>|  
|<span data-ttu-id="a45fc-129">**/BK: < BitLockerKey\>**</span><span class="sxs-lookup"><span data-stu-id="a45fc-129">**/bk:<BitLockerKey\>**</span></span>|<span data-ttu-id="a45fc-130">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="a45fc-130">Optional.</span></span> <span data-ttu-id="a45fc-131">Pokud chcete nástroj k odemknutí zašifrované uložení exportované soubory musíte zadat klíč nástroje BitLocker.</span><span class="sxs-lookup"><span data-stu-id="a45fc-131">You should specify the BitLocker key if you want the tool to unlock an encrypted where the exported files are stored.</span></span>|  
|<span data-ttu-id="a45fc-132">**/sn: < StorageAccountName\>**</span><span class="sxs-lookup"><span data-stu-id="a45fc-132">**/sn:<StorageAccountName\>**</span></span>|<span data-ttu-id="a45fc-133">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="a45fc-133">Required.</span></span> <span data-ttu-id="a45fc-134">Název účtu úložiště pro úlohy exportu.</span><span class="sxs-lookup"><span data-stu-id="a45fc-134">The name of the storage account for the export job.</span></span>|  
|<span data-ttu-id="a45fc-135">**/Sk: < StorageAccountKey\>**</span><span class="sxs-lookup"><span data-stu-id="a45fc-135">**/sk:<StorageAccountKey\>**</span></span>|<span data-ttu-id="a45fc-136">**Požadované** Pokud není zadán sdíleného přístupového podpisu kontejneru.</span><span class="sxs-lookup"><span data-stu-id="a45fc-136">**Required** if and only if a container SAS is not specified.</span></span> <span data-ttu-id="a45fc-137">Klíč účtu pro účet úložiště pro úlohy exportu.</span><span class="sxs-lookup"><span data-stu-id="a45fc-137">The account key for the storage account for the export job.</span></span>|  
|<span data-ttu-id="a45fc-138">**/csas: < ContainerSas\>**</span><span class="sxs-lookup"><span data-stu-id="a45fc-138">**/csas:<ContainerSas\>**</span></span>|<span data-ttu-id="a45fc-139">**Požadované** Pokud není zadaný klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="a45fc-139">**Required** if and only if the storage account key is not specified.</span></span> <span data-ttu-id="a45fc-140">Kontejner SAS pro přístup k objektům BLOB spojené s úlohou export.</span><span class="sxs-lookup"><span data-stu-id="a45fc-140">The container SAS for accessing the blobs associated with the export job.</span></span>|  
|<span data-ttu-id="a45fc-141">**/ CopyLogFile: < DriveCopyLogFile\>**</span><span class="sxs-lookup"><span data-stu-id="a45fc-141">**/CopyLogFile:<DriveCopyLogFile\>**</span></span>|<span data-ttu-id="a45fc-142">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="a45fc-142">Required.</span></span> <span data-ttu-id="a45fc-143">Cesta k souboru protokolu kopie disku.</span><span class="sxs-lookup"><span data-stu-id="a45fc-143">The path to the drive copy log file.</span></span> <span data-ttu-id="a45fc-144">Soubor je generována pomocí služby Windows Azure Import/Export a si můžete stáhnout z úložiště objektů blob, které jsou přidružené k úloze.</span><span class="sxs-lookup"><span data-stu-id="a45fc-144">The file is generated by the Windows Azure Import/Export service and can be downloaded from the blob storage associated with the job.</span></span> <span data-ttu-id="a45fc-145">Kopírovat soubor protokolu obsahuje informace o selhání objektů BLOB nebo soubory, které mají být opravit.</span><span class="sxs-lookup"><span data-stu-id="a45fc-145">The copy log file contains information about failed blobs or files which are to be repaired.</span></span>|  
|<span data-ttu-id="a45fc-146">**/ ManifestFile: < DriveManifestFile\>**</span><span class="sxs-lookup"><span data-stu-id="a45fc-146">**/ManifestFile:<DriveManifestFile\>**</span></span>|<span data-ttu-id="a45fc-147">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="a45fc-147">Optional.</span></span> <span data-ttu-id="a45fc-148">Cesta k souboru manifestu export jednotky.</span><span class="sxs-lookup"><span data-stu-id="a45fc-148">The path to the export drive's manifest file.</span></span> <span data-ttu-id="a45fc-149">Tento soubor je generována pomocí služby Windows Azure Import/Export a uložené na jednotce exportu a volitelně do objektu BLOB v účtu úložiště, které jsou přidružené k úloze.</span><span class="sxs-lookup"><span data-stu-id="a45fc-149">This file is generated by the Windows Azure Import/Export service and stored on the export drive, and optionally in a blob in the storage account associated with the job.</span></span><br /><br /> <span data-ttu-id="a45fc-150">Obsah souborů na jednotce exportu se ověří pomocí hodnot hash MD5, obsažené v tomto souboru.</span><span class="sxs-lookup"><span data-stu-id="a45fc-150">The content of the files on the export drive will be verified with the MD5 hashes contained in this file.</span></span> <span data-ttu-id="a45fc-151">Všechny soubory, které jsou určeny poškozen bude stažena a přepisuje, aby cílový adresáře.</span><span class="sxs-lookup"><span data-stu-id="a45fc-151">Any files that are determined to be corrupted will be downloaded and rewritten to the target directories.</span></span>|  
  
## <a name="using-repairexport-mode-to-correct-failed-exports"></a><span data-ttu-id="a45fc-152">Použití režimu RepairExport k opravě neúspěšné exporty</span><span class="sxs-lookup"><span data-stu-id="a45fc-152">Using RepairExport mode to correct failed exports</span></span>  
<span data-ttu-id="a45fc-153">Nástroj Azure Import/Export můžete stáhnout soubory, které se nepodařilo exportovat.</span><span class="sxs-lookup"><span data-stu-id="a45fc-153">You can use the Azure Import/Export Tool to download files that failed to export.</span></span> <span data-ttu-id="a45fc-154">Kopírování souboru protokolu bude obsahovat seznam souborů, které se nepodařilo exportovat.</span><span class="sxs-lookup"><span data-stu-id="a45fc-154">The copy log file will contain a list of files that failed to export.</span></span>  
  
<span data-ttu-id="a45fc-155">Příčiny chyb export patří tyto možnosti:</span><span class="sxs-lookup"><span data-stu-id="a45fc-155">The causes of export failures include the following possibilities:</span></span>  
  
-   <span data-ttu-id="a45fc-156">Poškozené jednotky</span><span class="sxs-lookup"><span data-stu-id="a45fc-156">Damaged drives</span></span>  
  
-   <span data-ttu-id="a45fc-157">Změnit během procesu přenosu klíče účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="a45fc-157">The storage account key changed during the transfer process</span></span>  
  
<span data-ttu-id="a45fc-158">Chcete-li spustit nástroj **RepairExport** režim, musíte nejdřív připojit jednotku, která obsahuje exportované soubory do počítače.</span><span class="sxs-lookup"><span data-stu-id="a45fc-158">To run the tool in **RepairExport** mode, you first need to connect the drive containing the exported files to your computer.</span></span> <span data-ttu-id="a45fc-159">Potom spusťte nástroj Azure Import/Export, zadání cesty na tuto jednotku s `/d` parametr.</span><span class="sxs-lookup"><span data-stu-id="a45fc-159">Next, run the Azure Import/Export Tool, specifying the path to that drive with the `/d` parameter.</span></span> <span data-ttu-id="a45fc-160">Také budete muset zadat cestu k souboru protokolu na jednotku kopie, který jste stáhli.</span><span class="sxs-lookup"><span data-stu-id="a45fc-160">You also need to specify the path to the drive's copy log file that you downloaded.</span></span> <span data-ttu-id="a45fc-161">Následující příklad následující příkazový řádek spustí nástroj a opravte všechny soubory, které se nepodařilo exportovat:</span><span class="sxs-lookup"><span data-stu-id="a45fc-161">The following command line example below runs the tool to repair any files that failed to export:</span></span>  
  
```  
WAImportExport.exe RepairExport /r:C:\WAImportExport\9WM35C3U.rep /d:G:\ /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C3U.log  
```  
  
<span data-ttu-id="a45fc-162">Toto je příklad kopírovat soubor protokolu, který ukazuje, že jeden blok v objektu blob selhalo při exportu:</span><span class="sxs-lookup"><span data-stu-id="a45fc-162">The following is an example of a copy log file that shows that one block in the blob failed to export:</span></span>  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveLog>  
  <DriveId>9WM35C2V</DriveId>  
  <Blob Status="CompletedWithErrors">  
    <BlobPath>pictures/wild/desert.jpg</BlobPath>  
    <FilePath>\pictures\wild\desert.jpg</FilePath>  
    <LastModified>2012-09-18T23:47:08Z</LastModified>  
    <Length>163840</Length>  
    <BlockList>  
      <Block Offset="65536" Length="65536" Id="AQAAAA==" Status="Failed" />  
    </BlockList>  
  </Blob>  
  <Status>CompletedWithErrors</Status>  
</DriveLog>  
```  
  
<span data-ttu-id="a45fc-163">Kopírování souboru protokolu označuje, že selhání došlo k chybě služby Windows Azure Import/Export byl stahování jeden objekt blob bloků k souboru na jednotce export.</span><span class="sxs-lookup"><span data-stu-id="a45fc-163">The copy log file indicates that a failure occurred while the Windows Azure Import/Export service was downloading one of the blob's blocks to the file on the export drive.</span></span> <span data-ttu-id="a45fc-164">Ostatní součásti souboru úspěšně stažen a délku souboru byla nastavena správně.</span><span class="sxs-lookup"><span data-stu-id="a45fc-164">The other components of the file downloaded successfully, and the file length was correctly set.</span></span> <span data-ttu-id="a45fc-165">V takovém případě nástroj se otevřít soubor na disku, stáhněte bloku z účtu úložiště a zapisovat do souboru rozsah od posun 65536 s délkou 65536.</span><span class="sxs-lookup"><span data-stu-id="a45fc-165">In this case, the tool will open the file on the drive, download the block from the storage account, and write it to the file range starting from offset 65536 with length 65536.</span></span>  
  
## <a name="using-repairexport-to-validate-drive-contents"></a><span data-ttu-id="a45fc-166">Pomocí RepairExport ověřit obsah jednotky</span><span class="sxs-lookup"><span data-stu-id="a45fc-166">Using RepairExport to validate drive contents</span></span>  
<span data-ttu-id="a45fc-167">Můžete také použít Azure Import/Export s **RepairExport** možnost k ověření obsahu na jednotce jsou správné.</span><span class="sxs-lookup"><span data-stu-id="a45fc-167">You can also use Azure Import/Export with the **RepairExport** option to validate the contents on the drive are correct.</span></span> <span data-ttu-id="a45fc-168">Soubor manifestu na každém disku exportu obsahuje MD5s pro obsah jednotky.</span><span class="sxs-lookup"><span data-stu-id="a45fc-168">The manifest file on each export drive contains MD5s for the contents of the drive.</span></span>  
  
<span data-ttu-id="a45fc-169">Služba Azure Import/Export můžete také na účet úložiště uložit soubory manifestu během procesu exportu.</span><span class="sxs-lookup"><span data-stu-id="a45fc-169">The Azure Import/Export service can also save the manifest files to a storage account during the export process.</span></span> <span data-ttu-id="a45fc-170">Umístění manifestu souborů je k dispozici prostřednictvím [Get Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operaci po dokončení úlohy.</span><span class="sxs-lookup"><span data-stu-id="a45fc-170">The location of the manifest files is available via the [Get Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operation when the job has completed.</span></span> <span data-ttu-id="a45fc-171">V tématu [službu Import/Export formát souboru manifestu](storage-import-export-file-format-metadata-and-properties.md) Další informace o formátu souboru manifestu jednotky.</span><span class="sxs-lookup"><span data-stu-id="a45fc-171">See [Import/Export service Manifest File Format](storage-import-export-file-format-metadata-and-properties.md) for more information about the format of a drive manifest file.</span></span>  
  
<span data-ttu-id="a45fc-172">Následující příklad ukazuje, jak spustit nástroj Azure Import/Export s **/ManifestFile** a **/CopyLogFile** parametry:</span><span class="sxs-lookup"><span data-stu-id="a45fc-172">The following example shows how to run the Azure Import/Export Tool with the **/ManifestFile** and **/CopyLogFile** parameters:</span></span>  
  
```  
WAImportExport.exe RepairExport /r:C:\WAImportExport\9WM35C3U.rep /d:G:\ /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C3U.log /ManifestFile:G:\9WM35C3U.manifest  
```  
  
<span data-ttu-id="a45fc-173">Následuje příklad souboru manifestu:</span><span class="sxs-lookup"><span data-stu-id="a45fc-173">The following is an example of a manifest file:</span></span>  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveManifest Version="2011-10-01">  
  <Drive>  
    <DriveId>9WM35C3U</DriveId>  
    <ClientCreator>Windows Azure Import/Export service</ClientCreator>  
    <BlobList>
      <Blob>  
        <BlobPath>pictures/city/redmond.jpg</BlobPath>  
        <FilePath>\pictures\city\redmond.jpg</FilePath>  
        <Length>15360</Length>  
        <PageRangeList>  
          <PageRange Offset="0" Length="3584" Hash="72FC55ED9AFDD40A0C8D5C4193208416" />  
          <PageRange Offset="3584" Length="3584" Hash="68B28A561B73D1DA769D4C24AA427DB8" />  
          <PageRange Offset="7168" Length="512" Hash="F521DF2F50C46BC5F9EA9FB787A23EED" />  
        </PageRangeList>  
        <PropertiesPath Hash="E72A22EA959566066AD89E3B49020C0A">\pictures\city\redmond.jpg.properties</PropertiesPath>  
      </Blob>  
      <Blob>  
        <BlobPath>pictures/wild/canyon.jpg</BlobPath>  
        <FilePath>\pictures\wild\canyon.jpg</FilePath>  
        <Length>10884</Length>  
        <BlockList>  
          <Block Offset="0" Length="2721" Id="AAAAAA==" Hash="263DC9C4B99C2177769C5EBE04787037" />  
          <Block Offset="2721" Length="2721" Id="AQAAAA==" Hash="0C52BAE2CC20EFEC15CC1E3045517AA6" />  
          <Block Offset="5442" Length="2721" Id="AgAAAA==" Hash="73D1CB62CB426230C34C9F57B7148F10" />  
          <Block Offset="8163" Length="2721" Id="AwAAAA==" Hash="11210E665C5F8E7E4F136D053B243E6A" />  
        </BlockList>  
        <PropertiesPath Hash="81D7F81B2C29F10D6E123D386C3A4D5A">\pictures\wild\canyon.jpg.properties</PropertiesPath>  
      </Blob> 
    </BlobList>  
 </Drive>  
</DriveManifest>  
``` 
  
<span data-ttu-id="a45fc-174">Po dokončení procesu opravy, bude nástroj přečíst každý soubor v souboru manifestu odkazuje a ověří integritu souboru s hodnoty hash MD5.</span><span class="sxs-lookup"><span data-stu-id="a45fc-174">After finishing the repair process, the tool will read through each file referenced in the manifest file and verify the file's integrity with the MD5 hashes.</span></span> <span data-ttu-id="a45fc-175">Pro manifest výše protože budou přenášeny pomocí následující součásti.</span><span class="sxs-lookup"><span data-stu-id="a45fc-175">For the manifest above, it will go through the following components.</span></span>  

```  
G:\pictures\city\redmond.jpg, offset 0, length 3584  
  
G:\pictures\city\redmond.jpg, offset 3584, length 3584  
  
G:\pictures\city\redmond.jpg, offset 7168, length 3584  
  
G:\pictures\city\redmond.jpg.properties  
  
G:\pictures\wild\canyon.jpg, offset 0, length 2721  
  
G:\pictures\wild\canyon.jpg, offset 2721, length 2721  
  
G:\pictures\wild\canyon.jpg, offset 5442, length 2721  
  
G:\pictures\wild\canyon.jpg, offset 8163, length 2721  
  
G:\pictures\wild\canyon.jpg.properties  
```

<span data-ttu-id="a45fc-176">Všechny součásti, které selhání ověření bude stažena nástrojem a přepsaná do stejného souboru na disku.</span><span class="sxs-lookup"><span data-stu-id="a45fc-176">Any component failing the verification will be downloaded by the tool and rewritten to the same file on the drive.</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="a45fc-177">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a45fc-177">Next steps</span></span>
 
* [<span data-ttu-id="a45fc-178">Nastavení nástroje Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="a45fc-178">Setting Up the Azure Import/Export Tool</span></span>](storage-import-export-tool-setup-v1.md)   
* [<span data-ttu-id="a45fc-179">Příprava pevných disků pro úlohu importu</span><span class="sxs-lookup"><span data-stu-id="a45fc-179">Preparing hard drives for an import job</span></span>](storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [<span data-ttu-id="a45fc-180">Kontrola stavu úlohy s použitím kopií souborů protokolu</span><span class="sxs-lookup"><span data-stu-id="a45fc-180">Reviewing job status with copy log files</span></span>](storage-import-export-tool-reviewing-job-status-v1.md)   
* [<span data-ttu-id="a45fc-181">Oprava úlohy importu</span><span class="sxs-lookup"><span data-stu-id="a45fc-181">Repairing an import job</span></span>](storage-import-export-tool-repairing-an-import-job-v1.md)   
* [<span data-ttu-id="a45fc-182">Řešení potíží s nástrojem Azure pro import/export</span><span class="sxs-lookup"><span data-stu-id="a45fc-182">Troubleshooting the Azure Import/Export Tool</span></span>](storage-import-export-tool-troubleshooting-v1.md)
