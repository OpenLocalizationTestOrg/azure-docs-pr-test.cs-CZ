---
title: "aaaSetting až hello nástroj Azure Import/Export | Microsoft Docs"
description: "Zjistěte, jak tooset až hello řídí přípravy a nástroje pro opravu pro službu Azure Import/Export hello."
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
ms.date: 06/29/2017
ms.author: muralikk
ms.openlocfilehash: 3d8897f2783112e07123669f1ba5b22576770e60
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="setting-up-hello-azure-importexport-tool"></a><span data-ttu-id="83f7b-103">Nastavení hello nástroj Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="83f7b-103">Setting up hello Azure Import/Export Tool</span></span>

<span data-ttu-id="83f7b-104">Hello nástroj Microsoft Azure Import/Export je hello jednotky přípravy a nástroje pro opravu, můžete použít s hello služby Microsoft Azure Import/Export.</span><span class="sxs-lookup"><span data-stu-id="83f7b-104">hello Microsoft Azure Import/Export Tool is hello drive preparation and repair tool that you can use with hello Microsoft Azure Import/Export service.</span></span> <span data-ttu-id="83f7b-105">Hello nástroj můžete použít pro hello následující funkce:</span><span class="sxs-lookup"><span data-stu-id="83f7b-105">You can use hello tool for hello following functions:</span></span>

* <span data-ttu-id="83f7b-106">Před vytvořením úlohy importu, můžete použít tento nástroj toocopy data toohello pevné disky budete tooship tooan datového centra Azure.</span><span class="sxs-lookup"><span data-stu-id="83f7b-106">Before creating an import job, you can use this tool toocopy data toohello hard drives you are going tooship tooan Azure data center.</span></span>
* <span data-ttu-id="83f7b-107">Po dokončení úlohy importu, můžete tento nástroj toorepair všechny objekty BLOB, které došlo k poškození, chyběl nebo konflikt s další objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="83f7b-107">After an import job has completed, you can use this tool toorepair any blobs that were corrupted, were missing, or conflicted with other blobs.</span></span>
* <span data-ttu-id="83f7b-108">Po přijetí hello jednotky z úlohy dokončené export, můžete použít tento nástroj toorepair všechny soubory, které došlo k poškození nebo na hello jednotky chybí.</span><span class="sxs-lookup"><span data-stu-id="83f7b-108">After you receive hello drives from a completed export job, you can use this tool toorepair any files that were corrupted or missing on hello drives.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="83f7b-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="83f7b-109">Prerequisites</span></span>

<span data-ttu-id="83f7b-110">Pokud jste **Příprava jednotky** úlohy importu, musí být splněny hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="83f7b-110">If you are **preparing drives** for an import job, hello following prerequisites must be met:</span></span>

* <span data-ttu-id="83f7b-111">Musí mít aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="83f7b-111">You must have an active Azure subscription.</span></span>
* <span data-ttu-id="83f7b-112">Vaše předplatné musí zahrnovat účet úložiště se soubory hello toostore dostatek dostupného místa budete tooimport.</span><span class="sxs-lookup"><span data-stu-id="83f7b-112">Your subscription must include a storage account with enough available space toostore hello files you are going tooimport.</span></span>
* <span data-ttu-id="83f7b-113">Je nutné alespoň jeden z hello přístupových klíčů k účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="83f7b-113">You need at least one of hello storage account access keys.</span></span>
* <span data-ttu-id="83f7b-114">Je nutné počítač (hello "kopie počítače") s Windows 7, Windows Server 2008 R2 nebo novější operační systém Windows, který je nainstalovaný.</span><span class="sxs-lookup"><span data-stu-id="83f7b-114">You need a computer (hello "copy machine") with Windows 7, Windows Server 2008 R2, or a newer Windows operating system installed.</span></span>
* <span data-ttu-id="83f7b-115">Hello rozhraní .NET Framework 4, musí být nainstalován na počítači kopie hello.</span><span class="sxs-lookup"><span data-stu-id="83f7b-115">hello .NET Framework 4 must be installed on hello copy machine.</span></span>
* <span data-ttu-id="83f7b-116">Na počítači hello kopie musí být povolen nástroj BitLocker.</span><span class="sxs-lookup"><span data-stu-id="83f7b-116">BitLocker must be enabled on hello copy machine.</span></span>
* <span data-ttu-id="83f7b-117">Je třeba použít jednu nebo více prázdný 3,5 pevné disky SATA připojen toohello kopie počítače.</span><span class="sxs-lookup"><span data-stu-id="83f7b-117">You need one or more empty 3.5-inch SATA hard drives connected toohello copy machine.</span></span>
* <span data-ttu-id="83f7b-118">Hello soubory, které máte v plánu tooimport musí být přístupné z počítače hello kopírování, ať už jsou sdílené síťové složky nebo místní pevný disk.</span><span class="sxs-lookup"><span data-stu-id="83f7b-118">hello files you plan tooimport must be accessible from hello copy machine, whether they are on a network share or a local hard drive.</span></span>

<span data-ttu-id="83f7b-119">Pokud se pokoušíte příliš**opravit importu** který částečně selhala, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="83f7b-119">If you are attempting too**repair an import** that has partially failed, you need:</span></span>

* <span data-ttu-id="83f7b-120">soubory protokolu kopie Hello</span><span class="sxs-lookup"><span data-stu-id="83f7b-120">hello copy log files</span></span>
* <span data-ttu-id="83f7b-121">klíč účtu úložiště Hello</span><span class="sxs-lookup"><span data-stu-id="83f7b-121">hello storage account key</span></span>

<span data-ttu-id="83f7b-122">Pokud se pokoušíte příliš**opravit exportu** který částečně selhala, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="83f7b-122">If you are attempting too**repair an export**  that has partially failed, you need:</span></span>

* <span data-ttu-id="83f7b-123">soubory protokolu kopie Hello</span><span class="sxs-lookup"><span data-stu-id="83f7b-123">hello copy log files</span></span>
* <span data-ttu-id="83f7b-124">Hello souborů manifestu (volitelné)</span><span class="sxs-lookup"><span data-stu-id="83f7b-124">hello manifest files (optional)</span></span>
* <span data-ttu-id="83f7b-125">klíč účtu úložiště Hello</span><span class="sxs-lookup"><span data-stu-id="83f7b-125">hello storage account key</span></span>

## <a name="installing-hello-azure-importexport-tool"></a><span data-ttu-id="83f7b-126">Instalace hello nástroj Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="83f7b-126">Installing hello Azure Import/Export Tool</span></span>

<span data-ttu-id="83f7b-127">První, [stáhnout hello nástroj Azure Import/Export](https://www.microsoft.com/download/details.aspx?id=55280) a rozbalte ho například tooa adresáře v počítači, `c:\WAImportExport`.</span><span class="sxs-lookup"><span data-stu-id="83f7b-127">First, [download hello Azure Import/Export Tool](https://www.microsoft.com/download/details.aspx?id=55280) and extract it tooa directory on your computer, for example `c:\WAImportExport`.</span></span>

<span data-ttu-id="83f7b-128">Hello nástroj Azure Import/Export sestává z hello následující soubory:</span><span class="sxs-lookup"><span data-stu-id="83f7b-128">hello Azure Import/Export Tool consists of hello following files:</span></span>

* <span data-ttu-id="83f7b-129">DataSet.csv</span><span class="sxs-lookup"><span data-stu-id="83f7b-129">dataset.csv</span></span>
* <span data-ttu-id="83f7b-130">driveset.csv</span><span class="sxs-lookup"><span data-stu-id="83f7b-130">driveset.csv</span></span>
* <span data-ttu-id="83f7b-131">hddid.dll</span><span class="sxs-lookup"><span data-stu-id="83f7b-131">hddid.dll</span></span>
* <span data-ttu-id="83f7b-132">Microsoft.Data.Services.Client.dll</span><span class="sxs-lookup"><span data-stu-id="83f7b-132">Microsoft.Data.Services.Client.dll</span></span>
* <span data-ttu-id="83f7b-133">Microsoft.WindowsAzure.Storage.dll</span><span class="sxs-lookup"><span data-stu-id="83f7b-133">Microsoft.WindowsAzure.Storage.dll</span></span>
* <span data-ttu-id="83f7b-134">Microsoft.WindowsAzure.Storage.pdb</span><span class="sxs-lookup"><span data-stu-id="83f7b-134">Microsoft.WindowsAzure.Storage.pdb</span></span>
* <span data-ttu-id="83f7b-135">Microsoft.WindowsAzure.Storage.xml</span><span class="sxs-lookup"><span data-stu-id="83f7b-135">Microsoft.WindowsAzure.Storage.xml</span></span>
* <span data-ttu-id="83f7b-136">WAImportExport.exe</span><span class="sxs-lookup"><span data-stu-id="83f7b-136">WAImportExport.exe</span></span>
* <span data-ttu-id="83f7b-137">WAImportExport.exe.config</span><span class="sxs-lookup"><span data-stu-id="83f7b-137">WAImportExport.exe.config</span></span>
* <span data-ttu-id="83f7b-138">WAImportExport.pdb</span><span class="sxs-lookup"><span data-stu-id="83f7b-138">WAImportExport.pdb</span></span>
* <span data-ttu-id="83f7b-139">WAImportExportCore.dll</span><span class="sxs-lookup"><span data-stu-id="83f7b-139">WAImportExportCore.dll</span></span>
* <span data-ttu-id="83f7b-140">WAImportExportCore.pdb</span><span class="sxs-lookup"><span data-stu-id="83f7b-140">WAImportExportCore.pdb</span></span>
* <span data-ttu-id="83f7b-141">WAImportExportRepair.dll</span><span class="sxs-lookup"><span data-stu-id="83f7b-141">WAImportExportRepair.dll</span></span>
* <span data-ttu-id="83f7b-142">WAImportExportRepair.pdb</span><span class="sxs-lookup"><span data-stu-id="83f7b-142">WAImportExportRepair.pdb</span></span>

<span data-ttu-id="83f7b-143">Dále otevřete okno příkazového řádku v **režimu správce**, a změny do adresáře hello obsahující hello rozbalené soubory.</span><span class="sxs-lookup"><span data-stu-id="83f7b-143">Next, open a Command Prompt window in **Administrator mode**, and change into hello directory containing hello extracted files.</span></span>

<span data-ttu-id="83f7b-144">toooutput Nápověda pro příkaz hello, spusťte nástroj hello (`WAImportExport.exe`) bez parametrů:</span><span class="sxs-lookup"><span data-stu-id="83f7b-144">toooutput help for hello command, run hello tool (`WAImportExport.exe`) without parameters:</span></span>

```
WAImportExport, a client tool for Windows Azure Import/Export Service. Microsoft (c) 2013


Copy directories and/or files with a new copy session:
    WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>]
        [/sk:<StorageAccountKey>] [/silentmode] [/InitialDriveSet:<driveset.csv>]
        DataSet:<dataset.csv>

Add more drives:
    WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /AdditionalDriveSet:<driveset.csv>

Abort an interrupted copy session:
    WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /AbortSession

Resume an interrupted copy session:
    WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /ResumeSession

List drives:
    WAImportExport.exe PrepImport /j:<JournalFile> /ListDrives

List copy sessions:
    WAImportExport.exe PrepImport /j:<JournalFile> /ListCopySessions

Repair a Drive:
    WAImportExport.exe RepairImport | RepairExport
        /r:<RepairFile> [/logdir:<LogDirectory>]
        [/d:<TargetDirectories>] [/bk:<BitLockerKey>]
        /sn:<StorageAccountName> /sk:<StorageAccountKey>
        [/CopyLogFile:<DriveCopyLogFile>] [/ManifestFile:<DriveManifestFile>]
        [/PathMapFile:<DrivePathMapFile>]

Preview an Export Job:
    WAImportExport.exe PreviewExport
        [/logdir:<LogDirectory>]
        /sn:<StorageAccountName> /sk:<StorageAccountKey>
        /ExportBlobListFile:<ExportBlobListFile> /DriveSize:<DriveSize>

Parameters:

    /j:<JournalFile>
        - Required. Path toohello journal file. A journal file tracks a set of drives and
          records hello progress in preparing these drives. hello journal file must always
          be specified.
    /logdir:<LogDirectory>
        - Optional. hello log directory. Verbose log files as well as some temporary
          files will be written toothis directory. If not specified, current directory
          will be used as hello log directory. hello log directory can be specified only
          once for hello same journal file.
    /id:<SessionId>
        - Optional. hello session Id is used tooidentify a copy session. It is used to
          ensure accurate recovery of an interrupted copy session.
    /ResumeSession
        - Optional. If hello last copy session was terminated abnormally, this parameter
          can be specified tooresume hello session.
    /AbortSession
        - Optional. If hello last copy session was terminated abnormally, this parameter
          can be specified tooabort hello session.
    /sn:<StorageAccountName>
        - Required. Only applicable for RepairImport and RepairExport. hello name of
          hello storage account.
    /sk:<StorageAccountKey>
        - Required. hello key of hello storage account.
    /InitialDriveSet:<driveset.csv>
        - Required. A .csv file that contains a list of drives tooprepare.
    /AdditionalDriveSet:<driveset.csv>
        - Required. A .csv file that contains a list of additional drives toobe added.
    /r:<RepairFile>
        - Required. Only applicable for RepairImport and RepairExport.
          Path toohello file for tracking repair progress. Each drive must have one
          and only one repair file.
    /d:<TargetDirectories>
        - Required. Only applicable for RepairImport and RepairExport.
          For RepairImport, one or more semicolon-separated directories toorepair;
          For RepairExport, one directory toorepair, e.g. root directory of hello drive.
    /CopyLogFile:<DriveCopyLogFile>
        - Required. Only applicable for RepairImport and RepairExport. Path toothe
          drive copy log file (verbose or error).
    /ManifestFile:<DriveManifestFile>
        - Required. Only applicable for RepairExport. Path toohello drive manifest file.
    /PathMapFile:<DrivePathMapFile>
        - Optional. Only applicable for RepairImport. Path toohello file containing
          mappings of file paths relative toohello drive root toolocations of actual files
          (tab-delimited). When first specified, it will be populated with file paths
          with empty targets, which means either they are not found in TargetDirectories,
          access denied, with invalid name, or they exist in multiple directories. The
          path map file can be manually edited tooinclude hello correct target paths and
          specified again for hello tool tooresolve hello file paths correctly.
    /ExportBlobListFile:<ExportBlobListFile>
        - Required. Path toohello XML file containing list of blob paths or blob path
          prefixes for hello blobs toobe exported. hello file format is hello same as the
          blob list blob format in hello Put Job operation of hello Import/Export Service
          REST API.
    /DriveSize:<DriveSize>
        - Required. Size of drives toobe used for export. For example, 500GB, 1.5TB.
          Note: 1 GB = 1,000,000,000 bytes
                1 TB = 1,000,000,000,000 bytes
    /DataSet:<dataset.csv>
        - Required. A .csv file that contains a list of directories and/or a list files
          toobe copied tootarget drives.

    /silentmode
        - Optional. If not specified, it will remind you hello requirement of drives and
          need your confirmation toocontinue.

Examples:

    Copy a data set tooa drive:
    WAImportExport.exe PrepImport
        /j:9WM35C2V.jrn /id:session#1 /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GEL
        xmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /InitialDriveSet:driveset1.csv
        /DataSet:data.csv

    Copy another dataset toohello same drive following hello above command:
    WAImportExport.exe PrepImport /j:9WM35C2V.jrn /id:session#2 /DataSet:dataset2.csv

    Preview how many 1.5 TB drives are needed for an export job:
    WAImportExport.exe PreviewExport
        /sn:mytestaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7K
        ysbbeKLDksg7VoN1W/a5UuM2zNgQ== /ExportBlobListFile:C:\temp\myexportbloblist.xml
        /DriveSize:1.5TB

    Repair an finished import job:
    WAImportExport.exe RepairImport
        /r:9WM35C2V.rep /d:X:\ /bk:442926-020713-108086-436744-137335-435358-242242-2795
        98 /sn:mytestaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94
        f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\temp\9WM35C2V_error.log
```

## <a name="next-steps"></a><span data-ttu-id="83f7b-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="83f7b-145">Next steps</span></span>

* [<span data-ttu-id="83f7b-146">Příprava pevných disků pro úlohu importu</span><span class="sxs-lookup"><span data-stu-id="83f7b-146">Preparing hard drives for an import job</span></span>](../storage-import-export-tool-preparing-hard-drives-import.md)
* [<span data-ttu-id="83f7b-147">Náhled využití disku pro úlohu exportu</span><span class="sxs-lookup"><span data-stu-id="83f7b-147">Previewing drive usage for an export job</span></span>](../storage-import-export-tool-previewing-drive-usage-export-v1.md)
* [<span data-ttu-id="83f7b-148">Kontrola stavu úlohy s použitím kopií souborů protokolu</span><span class="sxs-lookup"><span data-stu-id="83f7b-148">Reviewing job status with copy log files</span></span>](../storage-import-export-tool-reviewing-job-status-v1.md)
* [<span data-ttu-id="83f7b-149">Oprava úlohy importu</span><span class="sxs-lookup"><span data-stu-id="83f7b-149">Repairing an import job</span></span>](../storage-import-export-tool-repairing-an-import-job-v1.md)
* [<span data-ttu-id="83f7b-150">Oprava úlohy exportu</span><span class="sxs-lookup"><span data-stu-id="83f7b-150">Repairing an export job</span></span>](../storage-import-export-tool-repairing-an-export-job-v1.md)
* [<span data-ttu-id="83f7b-151">Řešení potíží s hello nástroj Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="83f7b-151">Troubleshooting hello Azure Import/Export Tool</span></span>](storage-import-export-tool-troubleshooting-v1.md)
