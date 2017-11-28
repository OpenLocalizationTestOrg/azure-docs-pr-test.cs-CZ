---
title: "aaaRepairing úlohy exportu Azure Import/Export - v1 | Microsoft Docs"
description: "Zjistěte, jak toorepair úlohu export, který byl vytvořen a spustí pomocí hello Azure Import/Export služby."
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
ms.openlocfilehash: 96c674fc7c697c37882fb2980c340303896ac6c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="repairing-an-export-job"></a><span data-ttu-id="94ab3-103">Oprava úlohy exportu</span><span class="sxs-lookup"><span data-stu-id="94ab3-103">Repairing an export job</span></span>
<span data-ttu-id="94ab3-104">Po dokončení úlohy exportu, můžete spustit hello Microsoft Azure Import/Export nástroj místně pro:</span><span class="sxs-lookup"><span data-stu-id="94ab3-104">After an export job has completed, you can run hello Microsoft Azure Import/Export Tool on-premises to:</span></span>  
  
1.  <span data-ttu-id="94ab3-105">Stáhněte všechny soubory, aby byla služba Azure Import/Export hello nelze tooexport.</span><span class="sxs-lookup"><span data-stu-id="94ab3-105">Download any files that hello Azure Import/Export service was unable tooexport.</span></span>  
  
2.  <span data-ttu-id="94ab3-106">Ověření, aby byly správně exportovány hello souborů na disku hello.</span><span class="sxs-lookup"><span data-stu-id="94ab3-106">Validate that hello files on hello drive were correctly exported.</span></span>  
  
<span data-ttu-id="94ab3-107">Tato funkce musí mít připojení k tooAzure úložiště toouse.</span><span class="sxs-lookup"><span data-stu-id="94ab3-107">You must have connectivity tooAzure Storage toouse this functionality.</span></span>  
  
<span data-ttu-id="94ab3-108">příkaz Hello pro opravu úlohy importu je **RepairExport**.</span><span class="sxs-lookup"><span data-stu-id="94ab3-108">hello command for repairing an import job is **RepairExport**.</span></span>

## <a name="repairexport-parameters"></a><span data-ttu-id="94ab3-109">Parametry RepairExport</span><span class="sxs-lookup"><span data-stu-id="94ab3-109">RepairExport parameters</span></span>

<span data-ttu-id="94ab3-110">Hello mohou být zadány následující parametry s **RepairExport**:</span><span class="sxs-lookup"><span data-stu-id="94ab3-110">hello following parameters can be specified with **RepairExport**:</span></span>  
  
|<span data-ttu-id="94ab3-111">Parametr</span><span class="sxs-lookup"><span data-stu-id="94ab3-111">Parameter</span></span>|<span data-ttu-id="94ab3-112">Popis</span><span class="sxs-lookup"><span data-stu-id="94ab3-112">Description</span></span>|  
|---------------|-----------------|  
|<span data-ttu-id="94ab3-113">**/ r: < RepairFile\>**</span><span class="sxs-lookup"><span data-stu-id="94ab3-113">**/r:<RepairFile\>**</span></span>|<span data-ttu-id="94ab3-114">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="94ab3-114">Required.</span></span> <span data-ttu-id="94ab3-115">Cesta toohello oprava souboru, který sleduje hello průběh hello opravy a můžete tooresume opravu dojde k přerušení.</span><span class="sxs-lookup"><span data-stu-id="94ab3-115">Path toohello repair file, which tracks hello progress of hello repair, and allows you tooresume an interrupted repair.</span></span> <span data-ttu-id="94ab3-116">Každé jednotky, musí mít pouze jeden soubor opravit.</span><span class="sxs-lookup"><span data-stu-id="94ab3-116">Each drive must have one and only one repair file.</span></span> <span data-ttu-id="94ab3-117">Když spustíte opravy pro danou jednotku, předáte bude v hello cesta tooa oprava souboru, který ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="94ab3-117">When you start a repair for a given drive, you will pass in hello path tooa repair file which does not yet exist.</span></span> <span data-ttu-id="94ab3-118">tooresume opravu dojde k přerušení, by měla předávat název hello k existujícímu souboru opravit.</span><span class="sxs-lookup"><span data-stu-id="94ab3-118">tooresume an interrupted repair, you should pass in hello name of an existing repair file.</span></span> <span data-ttu-id="94ab3-119">Hello opravit soubor odpovídající toohello cílová jednotka je vždy třeba zadat.</span><span class="sxs-lookup"><span data-stu-id="94ab3-119">hello repair file corresponding toohello target drive must always be specified.</span></span>|  
|<span data-ttu-id="94ab3-120">**/logdir: < LogDirectory\>**</span><span class="sxs-lookup"><span data-stu-id="94ab3-120">**/logdir:<LogDirectory\>**</span></span>|<span data-ttu-id="94ab3-121">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="94ab3-121">Optional.</span></span> <span data-ttu-id="94ab3-122">Adresář protokolu Hello.</span><span class="sxs-lookup"><span data-stu-id="94ab3-122">hello log directory.</span></span> <span data-ttu-id="94ab3-123">Podrobné soubory protokolu se zapíše toothis adresáře.</span><span class="sxs-lookup"><span data-stu-id="94ab3-123">Verbose log files will be written toothis directory.</span></span> <span data-ttu-id="94ab3-124">Pokud není zadán žádný adresář protokolu, aktuální adresář hello se použije jako adresář protokolu hello.</span><span class="sxs-lookup"><span data-stu-id="94ab3-124">If no log directory is specified, hello current directory will be used as hello log directory.</span></span>|  
|<span data-ttu-id="94ab3-125">**/ d: < TargetDirectory\>**</span><span class="sxs-lookup"><span data-stu-id="94ab3-125">**/d:<TargetDirectory\>**</span></span>|<span data-ttu-id="94ab3-126">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="94ab3-126">Required.</span></span> <span data-ttu-id="94ab3-127">Hello directory toovalidate a opravy.</span><span class="sxs-lookup"><span data-stu-id="94ab3-127">hello directory toovalidate and repair.</span></span> <span data-ttu-id="94ab3-128">To je obvykle hello kořenový adresář jednotky export hello, ale může také být do síťové sdílené složky obsahující kopii hello exportované soubory.</span><span class="sxs-lookup"><span data-stu-id="94ab3-128">This is usually hello root directory of hello export drive, but could also be a network file share containing a copy of hello exported files.</span></span>|  
|<span data-ttu-id="94ab3-129">**/BK: < BitLockerKey\>**</span><span class="sxs-lookup"><span data-stu-id="94ab3-129">**/bk:<BitLockerKey\>**</span></span>|<span data-ttu-id="94ab3-130">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="94ab3-130">Optional.</span></span> <span data-ttu-id="94ab3-131">Pokud chcete, aby toounlock nástroj hello šifrované kdy hello exportovaných souborů jsou uložené musíte zadat klíč nástroje BitLocker hello.</span><span class="sxs-lookup"><span data-stu-id="94ab3-131">You should specify hello BitLocker key if you want hello tool toounlock an encrypted where hello exported files are stored.</span></span>|  
|<span data-ttu-id="94ab3-132">**/sn: < StorageAccountName\>**</span><span class="sxs-lookup"><span data-stu-id="94ab3-132">**/sn:<StorageAccountName\>**</span></span>|<span data-ttu-id="94ab3-133">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="94ab3-133">Required.</span></span> <span data-ttu-id="94ab3-134">Úloha exportu Hello název účtu úložiště hello hello.</span><span class="sxs-lookup"><span data-stu-id="94ab3-134">hello name of hello storage account for hello export job.</span></span>|  
|<span data-ttu-id="94ab3-135">**/Sk: < StorageAccountKey\>**</span><span class="sxs-lookup"><span data-stu-id="94ab3-135">**/sk:<StorageAccountKey\>**</span></span>|<span data-ttu-id="94ab3-136">**Požadované** Pokud není zadán sdíleného přístupového podpisu kontejneru.</span><span class="sxs-lookup"><span data-stu-id="94ab3-136">**Required** if and only if a container SAS is not specified.</span></span> <span data-ttu-id="94ab3-137">Úloha exportu Hello klíč účtu pro účet úložiště hello hello.</span><span class="sxs-lookup"><span data-stu-id="94ab3-137">hello account key for hello storage account for hello export job.</span></span>|  
|<span data-ttu-id="94ab3-138">**/csas: < ContainerSas\>**</span><span class="sxs-lookup"><span data-stu-id="94ab3-138">**/csas:<ContainerSas\>**</span></span>|<span data-ttu-id="94ab3-139">**Požadované** jenom v případě hello klíč účtu úložiště není zadán.</span><span class="sxs-lookup"><span data-stu-id="94ab3-139">**Required** if and only if hello storage account key is not specified.</span></span> <span data-ttu-id="94ab3-140">kontejner Hello SAS pro přístup k objektům BLOB hello spojené s úlohou export hello.</span><span class="sxs-lookup"><span data-stu-id="94ab3-140">hello container SAS for accessing hello blobs associated with hello export job.</span></span>|  
|<span data-ttu-id="94ab3-141">**/ CopyLogFile: < DriveCopyLogFile\>**</span><span class="sxs-lookup"><span data-stu-id="94ab3-141">**/CopyLogFile:<DriveCopyLogFile\>**</span></span>|<span data-ttu-id="94ab3-142">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="94ab3-142">Required.</span></span> <span data-ttu-id="94ab3-143">Hello cesta toohello jednotky kopírování souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="94ab3-143">hello path toohello drive copy log file.</span></span> <span data-ttu-id="94ab3-144">soubor Hello je generován hello služby Windows Azure Import/Export a si můžete stáhnout z úložiště objektů blob hello spojené s úlohou hello.</span><span class="sxs-lookup"><span data-stu-id="94ab3-144">hello file is generated by hello Windows Azure Import/Export service and can be downloaded from hello blob storage associated with hello job.</span></span> <span data-ttu-id="94ab3-145">Hello kopie protokolový soubor obsahuje informace o selhání objektů BLOB nebo soubory, které jsou toobe opravit.</span><span class="sxs-lookup"><span data-stu-id="94ab3-145">hello copy log file contains information about failed blobs or files which are toobe repaired.</span></span>|  
|<span data-ttu-id="94ab3-146">**/ ManifestFile: < DriveManifestFile\>**</span><span class="sxs-lookup"><span data-stu-id="94ab3-146">**/ManifestFile:<DriveManifestFile\>**</span></span>|<span data-ttu-id="94ab3-147">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="94ab3-147">Optional.</span></span> <span data-ttu-id="94ab3-148">Soubor manifestu Hello cesta toohello export jednotky.</span><span class="sxs-lookup"><span data-stu-id="94ab3-148">hello path toohello export drive's manifest file.</span></span> <span data-ttu-id="94ab3-149">Tento soubor je vygenerované službou Windows Azure Import/Export hello a uložené na jednotce export hello a volitelně do objektu BLOB v účtu úložiště hello spojené s úlohou hello.</span><span class="sxs-lookup"><span data-stu-id="94ab3-149">This file is generated by hello Windows Azure Import/Export service and stored on hello export drive, and optionally in a blob in hello storage account associated with hello job.</span></span><br /><br /> <span data-ttu-id="94ab3-150">obsah hello souborů na jednotce export hello Hello se ověří pomocí hodnot hash MD5 hello obsažené v tomto souboru.</span><span class="sxs-lookup"><span data-stu-id="94ab3-150">hello content of hello files on hello export drive will be verified with hello MD5 hashes contained in this file.</span></span> <span data-ttu-id="94ab3-151">Všechny soubory, které jsou určené toobe poškozený budou staženy a přepsaná toohello cíl adresáře.</span><span class="sxs-lookup"><span data-stu-id="94ab3-151">Any files that are determined toobe corrupted will be downloaded and rewritten toohello target directories.</span></span>|  
  
## <a name="using-repairexport-mode-toocorrect-failed-exports"></a><span data-ttu-id="94ab3-152">Pomocí RepairExport toocorrect režimu se nezdařilo exporty</span><span class="sxs-lookup"><span data-stu-id="94ab3-152">Using RepairExport mode toocorrect failed exports</span></span>  
<span data-ttu-id="94ab3-153">Můžete použít hello nástroj Azure Import/Export toodownload soubory, které tooexport se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="94ab3-153">You can use hello Azure Import/Export Tool toodownload files that failed tooexport.</span></span> <span data-ttu-id="94ab3-154">soubor protokolu Hello kopie bude obsahovat seznam souborů, které se nezdařily tooexport.</span><span class="sxs-lookup"><span data-stu-id="94ab3-154">hello copy log file will contain a list of files that failed tooexport.</span></span>  
  
<span data-ttu-id="94ab3-155">Hello selhání exportu příčiny hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="94ab3-155">hello causes of export failures include hello following possibilities:</span></span>  
  
-   <span data-ttu-id="94ab3-156">Poškozené jednotky</span><span class="sxs-lookup"><span data-stu-id="94ab3-156">Damaged drives</span></span>  
  
-   <span data-ttu-id="94ab3-157">klíč účtu úložiště Hello změnit během přenosu hello</span><span class="sxs-lookup"><span data-stu-id="94ab3-157">hello storage account key changed during hello transfer process</span></span>  
  
<span data-ttu-id="94ab3-158">Nástroj hello toorun v **RepairExport** režim, musíte nejprve tooconnect hello jednotce, která obsahuje počítače tooyour hello exportované soubory.</span><span class="sxs-lookup"><span data-stu-id="94ab3-158">toorun hello tool in **RepairExport** mode, you first need tooconnect hello drive containing hello exported files tooyour computer.</span></span> <span data-ttu-id="94ab3-159">Potom spustíte hello nástroj Azure Import/Export, zadáním hello cesta toothat jednotky s hello `/d` parametr.</span><span class="sxs-lookup"><span data-stu-id="94ab3-159">Next, run hello Azure Import/Export Tool, specifying hello path toothat drive with hello `/d` parameter.</span></span> <span data-ttu-id="94ab3-160">Musíte taky toospecify hello cesta toohello jednotku na kopírování protokolu souboru, který jste stáhli.</span><span class="sxs-lookup"><span data-stu-id="94ab3-160">You also need toospecify hello path toohello drive's copy log file that you downloaded.</span></span> <span data-ttu-id="94ab3-161">Hello následující příkazový řádek příklad spustí nástroj toorepair hello všechny soubory, které se nezdařilo tooexport:</span><span class="sxs-lookup"><span data-stu-id="94ab3-161">hello following command line example below runs hello tool toorepair any files that failed tooexport:</span></span>  
  
```  
WAImportExport.exe RepairExport /r:C:\WAImportExport\9WM35C3U.rep /d:G:\ /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C3U.log  
```  
  
<span data-ttu-id="94ab3-162">Hello následuje příklad kopírování protokolového souboru, který zobrazuje tento jeden blok v tooexport hello objekt blob se nezdařilo:</span><span class="sxs-lookup"><span data-stu-id="94ab3-162">hello following is an example of a copy log file that shows that one block in hello blob failed tooexport:</span></span>  
  
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
  
<span data-ttu-id="94ab3-163">soubor protokolu kopie Hello označuje, že došlo k selhání při hello služby Windows Azure Import/Export byl stahování mezi hello blob bloků toohello soubor na disku export hello.</span><span class="sxs-lookup"><span data-stu-id="94ab3-163">hello copy log file indicates that a failure occurred while hello Windows Azure Import/Export service was downloading one of hello blob's blocks toohello file on hello export drive.</span></span> <span data-ttu-id="94ab3-164">Hello ostatní součásti hello soubor stažený úspěšně, a délka souboru hello byla správně nastavená.</span><span class="sxs-lookup"><span data-stu-id="94ab3-164">hello other components of hello file downloaded successfully, and hello file length was correctly set.</span></span> <span data-ttu-id="94ab3-165">Nástroj hello budou v takovém případě otevřete hello soubor na disku hello, stáhněte hello bloku z účtu úložiště hello a zapisuje toohello souboru rozsah od posun 65536 s délkou 65536.</span><span class="sxs-lookup"><span data-stu-id="94ab3-165">In this case, hello tool will open hello file on hello drive, download hello block from hello storage account, and write it toohello file range starting from offset 65536 with length 65536.</span></span>  
  
## <a name="using-repairexport-toovalidate-drive-contents"></a><span data-ttu-id="94ab3-166">Pomocí RepairExport toovalidate jednotky obsah</span><span class="sxs-lookup"><span data-stu-id="94ab3-166">Using RepairExport toovalidate drive contents</span></span>  
<span data-ttu-id="94ab3-167">Můžete také použít Azure Import/Export s hello **RepairExport** možnost toovalidate hello obsah na jednotce hello jsou správné.</span><span class="sxs-lookup"><span data-stu-id="94ab3-167">You can also use Azure Import/Export with hello **RepairExport** option toovalidate hello contents on hello drive are correct.</span></span> <span data-ttu-id="94ab3-168">Soubor manifestu Hello na každém disku exportu obsahuje MD5s pro obsah hello hello jednotky.</span><span class="sxs-lookup"><span data-stu-id="94ab3-168">hello manifest file on each export drive contains MD5s for hello contents of hello drive.</span></span>  
  
<span data-ttu-id="94ab3-169">Hello služba Azure Import/Export můžete také ukládat soubory manifestu hello účet úložiště tooa během procesu exportu hello.</span><span class="sxs-lookup"><span data-stu-id="94ab3-169">hello Azure Import/Export service can also save hello manifest files tooa storage account during hello export process.</span></span> <span data-ttu-id="94ab3-170">Hello umístění souborů manifestu hello je k dispozici prostřednictvím hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operaci po dokončení úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="94ab3-170">hello location of hello manifest files is available via hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operation when hello job has completed.</span></span> <span data-ttu-id="94ab3-171">V tématu [službu Import/Export formát souboru manifestu](storage-import-export-file-format-metadata-and-properties.md) Další informace o hello formát souboru manifestu jednotky.</span><span class="sxs-lookup"><span data-stu-id="94ab3-171">See [Import/Export service Manifest File Format](storage-import-export-file-format-metadata-and-properties.md) for more information about hello format of a drive manifest file.</span></span>  
  
<span data-ttu-id="94ab3-172">Hello následující příklad ukazuje, jak toorun hello Azure Import/Export nástroj s hello **/ManifestFile** a **/CopyLogFile** parametry:</span><span class="sxs-lookup"><span data-stu-id="94ab3-172">hello following example shows how toorun hello Azure Import/Export Tool with hello **/ManifestFile** and **/CopyLogFile** parameters:</span></span>  
  
```  
WAImportExport.exe RepairExport /r:C:\WAImportExport\9WM35C3U.rep /d:G:\ /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C3U.log /ManifestFile:G:\9WM35C3U.manifest  
```  
  
<span data-ttu-id="94ab3-173">Hello tady je příklad souboru manifestu:</span><span class="sxs-lookup"><span data-stu-id="94ab3-173">hello following is an example of a manifest file:</span></span>  
  
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
  
<span data-ttu-id="94ab3-174">Po dokončení procesu opravy hello bude nástroj hello si přečíst každý soubor v souboru manifestu hello odkazuje a ověří integritu souboru hello s hello MD5 hodnoty hash.</span><span class="sxs-lookup"><span data-stu-id="94ab3-174">After finishing hello repair process, hello tool will read through each file referenced in hello manifest file and verify hello file's integrity with hello MD5 hashes.</span></span> <span data-ttu-id="94ab3-175">Pro manifest hello výše protože budou přenášeny prostřednictvím hello následující součásti.</span><span class="sxs-lookup"><span data-stu-id="94ab3-175">For hello manifest above, it will go through hello following components.</span></span>  

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

<span data-ttu-id="94ab3-176">Všechny součásti, které selhání ověření hello bude stažena nástrojem hello a přepsaná toohello stejný soubor na disku hello.</span><span class="sxs-lookup"><span data-stu-id="94ab3-176">Any component failing hello verification will be downloaded by hello tool and rewritten toohello same file on hello drive.</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="94ab3-177">Další kroky</span><span class="sxs-lookup"><span data-stu-id="94ab3-177">Next steps</span></span>
 
* [<span data-ttu-id="94ab3-178">Hello nastavení se nástroj Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="94ab3-178">Setting Up hello Azure Import/Export Tool</span></span>](storage-import-export-tool-setup-v1.md)   
* [<span data-ttu-id="94ab3-179">Příprava pevných disků pro úlohu importu</span><span class="sxs-lookup"><span data-stu-id="94ab3-179">Preparing hard drives for an import job</span></span>](storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [<span data-ttu-id="94ab3-180">Kontrola stavu úlohy s použitím kopií souborů protokolu</span><span class="sxs-lookup"><span data-stu-id="94ab3-180">Reviewing job status with copy log files</span></span>](storage-import-export-tool-reviewing-job-status-v1.md)   
* [<span data-ttu-id="94ab3-181">Oprava úlohy importu</span><span class="sxs-lookup"><span data-stu-id="94ab3-181">Repairing an import job</span></span>](storage-import-export-tool-repairing-an-import-job-v1.md)   
* [<span data-ttu-id="94ab3-182">Řešení potíží s hello nástroj Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="94ab3-182">Troubleshooting hello Azure Import/Export Tool</span></span>](storage-import-export-tool-troubleshooting-v1.md)
