---
title: "Formát souboru protokolu Azure Import/Export | Microsoft Docs"
description: "Další informace o formátu souborů protokolu, který je vytvořen při provádění kroků pro úlohu importu/exportu služby."
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
ms.openlocfilehash: 16234ccaf13ce1d85cfd207ed4734e683070faa6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-importexport-service-log-file-format"></a><span data-ttu-id="9ae1b-103">Formát souboru protokolu služby sady Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="9ae1b-103">Azure Import/Export service log file format</span></span>
<span data-ttu-id="9ae1b-104">Když službu Microsoft Azure Import/Export provede akci na disku jako součást úlohy importu nebo úlohy exportu, protokoly zapisují na blok objektů BLOB v účtu úložiště přidruženého k této úlohy.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-104">When the Microsoft Azure Import/Export service performs an action on a drive as part of an import job or an export job, logs are written to block blobs in the storage account associated with that job.</span></span>  
  
<span data-ttu-id="9ae1b-105">Existují dva protokoly, které lze zapisovat pomocí služby importu a exportu:</span><span class="sxs-lookup"><span data-stu-id="9ae1b-105">There are two logs that may be written by the Import/Export service:</span></span>  
  
-   <span data-ttu-id="9ae1b-106">V případě chyby vždy generování protokolu chyb.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-106">The error log is always generated in the event of an error.</span></span>  
  
-   <span data-ttu-id="9ae1b-107">Úplné protokolování není ve výchozím nastavení povolené, ale lze ho zapnout nastavením `EnableVerboseLog` vlastnost [Put úlohy](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) nebo [vlastnosti úlohy aktualizace](/rest/api/storageimportexport/jobs#Jobs_Update) operaci.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-107">The verbose log is not enabled by default, but may be enabled by setting the `EnableVerboseLog` property on a [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) or [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) operation.</span></span>  
  
## <a name="log-file-location"></a><span data-ttu-id="9ae1b-108">Umístění souboru protokolu</span><span class="sxs-lookup"><span data-stu-id="9ae1b-108">Log file location</span></span>  
<span data-ttu-id="9ae1b-109">Protokoly se zapisují do bloku v kontejneru nebo virtuální adresář zadaný `ImportExportStatesPath` nastavení, které můžete nastavit `Put Job` operaci.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-109">The logs are written to block blobs in the container or virtual directory specified by the `ImportExportStatesPath` setting, which you can set on a `Put Job` operation.</span></span> <span data-ttu-id="9ae1b-110">Umístění, do které se zapisují protokoly závisí na tom, jak je ověřování zadaná pro úlohu, společně s hodnota zadaná pro `ImportExportStatesPath`.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-110">The location to which the logs are written depends on how authentication is specified for the job, together with the value specified for `ImportExportStatesPath`.</span></span> <span data-ttu-id="9ae1b-111">Ověření úlohy je možné zadat prostřednictvím klíč účtu úložiště nebo kontejneru SAS (sdílený přístupový podpis).</span><span class="sxs-lookup"><span data-stu-id="9ae1b-111">Authentication for the job may be specified via a storage account key, or a container SAS (shared access signature).</span></span>  
  
<span data-ttu-id="9ae1b-112">Název kontejneru nebo virtuální adresář může být výchozí název `waimportexport`, nebo jiný kontejner nebo název virtuálního adresáře, který určíte.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-112">The name of the container or virtual directory may either be the default name of `waimportexport`, or another container or virtual directory name that you specify.</span></span>  
  
<span data-ttu-id="9ae1b-113">Následující tabulka uvádí možné možnosti:</span><span class="sxs-lookup"><span data-stu-id="9ae1b-113">The table below shows the possible options:</span></span>  
  
|<span data-ttu-id="9ae1b-114">Metoda ověřování</span><span class="sxs-lookup"><span data-stu-id="9ae1b-114">Authentication Method</span></span>|<span data-ttu-id="9ae1b-115">Hodnota `ImportExportStatesPath`– Element</span><span class="sxs-lookup"><span data-stu-id="9ae1b-115">Value of `ImportExportStatesPath`Element</span></span>|<span data-ttu-id="9ae1b-116">Umístění protokolu objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="9ae1b-116">Location of Log Blobs</span></span>|  
|---------------------------|----------------------------------------------|---------------------------|  
|<span data-ttu-id="9ae1b-117">Klíče účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-117">Storage account key</span></span>|<span data-ttu-id="9ae1b-118">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="9ae1b-118">Default value</span></span>|<span data-ttu-id="9ae1b-119">Kontejner s názvem `waimportexport`, což je výchozí kontejner.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-119">A container named `waimportexport`, which is the default container.</span></span> <span data-ttu-id="9ae1b-120">Například:</span><span class="sxs-lookup"><span data-stu-id="9ae1b-120">For example:</span></span><br /><br /> `https://myaccount.blob.core.windows.net/waimportexport`|  
|<span data-ttu-id="9ae1b-121">Klíče účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-121">Storage account key</span></span>|<span data-ttu-id="9ae1b-122">Uživatelem zadanou hodnotu</span><span class="sxs-lookup"><span data-stu-id="9ae1b-122">User-specified value</span></span>|<span data-ttu-id="9ae1b-123">Kontejner s názvem uživatelem.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-123">A container named by the user.</span></span> <span data-ttu-id="9ae1b-124">Například:</span><span class="sxs-lookup"><span data-stu-id="9ae1b-124">For example:</span></span><br /><br /> `https://myaccount.blob.core.windows.net/mylogcontainer`|  
|<span data-ttu-id="9ae1b-125">Sdíleného přístupového podpisu kontejneru</span><span class="sxs-lookup"><span data-stu-id="9ae1b-125">Container SAS</span></span>|<span data-ttu-id="9ae1b-126">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="9ae1b-126">Default value</span></span>|<span data-ttu-id="9ae1b-127">Virtuální adresář s názvem `waimportexport`, což je výchozí název, pod kontejnerem zadaný v SAS.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-127">A virtual directory named `waimportexport`, which is the default name, beneath the container specified in the SAS.</span></span><br /><br /> <span data-ttu-id="9ae1b-128">Například pokud SAS zadaná pro úlohu je `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, pak by byl umístění protokolu`https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport`</span><span class="sxs-lookup"><span data-stu-id="9ae1b-128">For example, if the SAS specified for the job is  `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, then the log location would be `https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport`</span></span>|  
|<span data-ttu-id="9ae1b-129">Sdíleného přístupového podpisu kontejneru</span><span class="sxs-lookup"><span data-stu-id="9ae1b-129">Container SAS</span></span>|<span data-ttu-id="9ae1b-130">Uživatelem zadanou hodnotu</span><span class="sxs-lookup"><span data-stu-id="9ae1b-130">User-specified value</span></span>|<span data-ttu-id="9ae1b-131">Virtuální adresář s názvem uživatelem pod kontejnerem zadaný v SAS.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-131">A virtual directory named by the user, beneath the container specified in the SAS.</span></span><br /><br /> <span data-ttu-id="9ae1b-132">Například pokud SAS zadaná pro úlohu je `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, a názvem zadaný virtuální adresář `mylogblobs`, pak by byl umístění protokolu `https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport/mylogblobs`.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-132">For example, if the SAS specified for the job is  `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, and the specified virtual directory is named `mylogblobs`, then the log location would be `https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport/mylogblobs`.</span></span>|  
  
<span data-ttu-id="9ae1b-133">Je-li získat adresu URL pro podrobné protokoly a chyba pomocí volání [Get Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operaci.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-133">You can retrieve the URL for the error and verbose logs by calling the [Get Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operation.</span></span> <span data-ttu-id="9ae1b-134">Protokoly jsou k dispozici po dokončení zpracování jednotky.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-134">The logs are available after processing of the drive is complete.</span></span>  
  
## <a name="log-file-format"></a><span data-ttu-id="9ae1b-135">Formát souboru protokolu</span><span class="sxs-lookup"><span data-stu-id="9ae1b-135">Log file format</span></span>  
<span data-ttu-id="9ae1b-136">Formát pro oba protokoly je stejný: Objekt blob, který obsahuje XML popisy událostí, které nastaly při kopírování objekty BLOB mezi pevného disku a účtu zákazníka.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-136">The format for both logs is the same: a blob containing XML descriptions of the events that occurred while copying blobs between the hard drive and the customer's account.</span></span>  
  
<span data-ttu-id="9ae1b-137">Úplné protokolování obsahuje kompletní informace o stavu operace kopírování pro každý objekt blob (pro úlohy importu) nebo soubor (pro úlohy exportu), zatímco v protokolu chyb obsahuje pouze informace pro objekty BLOB nebo soubory, které se vyskytly chyby při importu nebo exportu úloze.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-137">The verbose log contains complete information about the status of the copy operation for every blob (for an import job) or file (for an export job), whereas the error log contains only the information for blobs or files that encountered errors during the import or export job.</span></span>  
  
<span data-ttu-id="9ae1b-138">Formát podrobného protokolování jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-138">The verbose log format is shown below.</span></span> <span data-ttu-id="9ae1b-139">V protokolu chyb má stejnou strukturu, ale filtruje úspěšné operace.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-139">The error log has the same structure, but filters out successful operations.</span></span>  

```xml
<DriveLog Version="2014-11-01">  
  <DriveId>drive-id</DriveId>  
  [<Blob Status="blob-status">  
   <BlobPath>blob-path</BlobPath>  
   <FilePath>file-path</FilePath>  
   [<Snapshot>snapshot</Snapshot>]  
   <Length>length</Length>  
   [<LastModified>last-modified</LastModified>]  
   [<ImportDisposition Status="import-disposition-status">import-disposition</ImportDisposition>]  
   [page-range-list-or-block-list]  
   [metadata-status]  
   [properties-status]  
  </Blob>]  
  [<Blob>  
    . . .  
  </Blob>]  
  <Status>drive-status</Status>  
</DriveLog>  
  
page-range-list-or-block-list ::= 
  page-range-list | block-list  
  
page-range-list ::=   
<PageRangeList>  
      [<PageRange Offset="page-range-offset" Length="page-range-length"   
       [Hash="md5-hash"] Status="page-range-status"/>]  
      [<PageRange Offset="page-range-offset" Length="page-range-length"   
       [Hash="md5-hash"] Status="page-range-status"/>]  
</PageRangeList>  
  
block-list ::=  
<BlockList>  
      [<Block Offset="block-offset" Length="block-length" [Id="block-id"]  
       [Hash="md5-hash"] Status="block-status"/>]  
      [<Block Offset="block-offset" Length="block-length" [Id="block-id"]   
       [Hash="md5-hash"] Status="block-status"/>]  
</BlockList>  
  
metadata-status ::=  
<Metadata Status="metadata-status">  
   [<GlobalPath Hash="md5-hash">global-metadata-file-path</GlobalPath>]  
   [<Path Hash="md5-hash">metadata-file-path</Path>]  
</Metadata>  
  
properties-status ::=  
<Properties Status="properties-status">  
   [<GlobalPath Hash="md5-hash">global-properties-file-path</GlobalPath>]  
   [<Path Hash="md5-hash">properties-file-path</Path>]  
</Properties>  
```

<span data-ttu-id="9ae1b-140">Následující tabulka popisuje prvky souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-140">The following table describes the elements of the log file.</span></span>  
  
|<span data-ttu-id="9ae1b-141">XML Element</span><span class="sxs-lookup"><span data-stu-id="9ae1b-141">XML Element</span></span>|<span data-ttu-id="9ae1b-142">Typ</span><span class="sxs-lookup"><span data-stu-id="9ae1b-142">Type</span></span>|<span data-ttu-id="9ae1b-143">Popis</span><span class="sxs-lookup"><span data-stu-id="9ae1b-143">Description</span></span>|  
|-----------------|----------|-----------------|  
|`DriveLog`|<span data-ttu-id="9ae1b-144">XML Element</span><span class="sxs-lookup"><span data-stu-id="9ae1b-144">XML Element</span></span>|<span data-ttu-id="9ae1b-145">Představuje jednotku protokolu.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-145">Represents a drive log.</span></span>|  
|`Version`|<span data-ttu-id="9ae1b-146">Atribut, řetězec</span><span class="sxs-lookup"><span data-stu-id="9ae1b-146">Attribute, String</span></span>|<span data-ttu-id="9ae1b-147">Verze formátu protokolu.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-147">The version of the log format.</span></span>|  
|`DriveId`|<span data-ttu-id="9ae1b-148">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9ae1b-148">String</span></span>|<span data-ttu-id="9ae1b-149">Sériové číslo hardwaru na jednotku.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-149">The drive's hardware serial number.</span></span>|  
|`Status`|<span data-ttu-id="9ae1b-150">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9ae1b-150">String</span></span>|<span data-ttu-id="9ae1b-151">Stav zpracování jednotky.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-151">Status of the drive processing.</span></span> <span data-ttu-id="9ae1b-152">Najdete v článku `Drive Status Codes` tabulky níže Další informace.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-152">See the `Drive Status Codes` table below for more information.</span></span>|  
|`Blob`|<span data-ttu-id="9ae1b-153">Vnořené – element XML</span><span class="sxs-lookup"><span data-stu-id="9ae1b-153">Nested XML element</span></span>|<span data-ttu-id="9ae1b-154">Představuje objekt blob.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-154">Represents a blob.</span></span>|  
|`Blob/BlobPath`|<span data-ttu-id="9ae1b-155">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9ae1b-155">String</span></span>|<span data-ttu-id="9ae1b-156">Identifikátor URI objektu blob.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-156">The URI of the blob.</span></span>|  
|`Blob/FilePath`|<span data-ttu-id="9ae1b-157">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9ae1b-157">String</span></span>|<span data-ttu-id="9ae1b-158">Relativní cesta k souboru na disku.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-158">The relative path to the file on the drive.</span></span>|  
|`Blob/Snapshot`|<span data-ttu-id="9ae1b-159">Data a času</span><span class="sxs-lookup"><span data-stu-id="9ae1b-159">DateTime</span></span>|<span data-ttu-id="9ae1b-160">Snímek aktuální verze objektu blob pro úlohu exportu.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-160">The snapshot version of the blob, for an export job only.</span></span>|  
|`Blob/Length`|<span data-ttu-id="9ae1b-161">Integer</span><span class="sxs-lookup"><span data-stu-id="9ae1b-161">Integer</span></span>|<span data-ttu-id="9ae1b-162">Celková délka objektu blob v bajtech.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-162">The total length of the blob in bytes.</span></span>|  
|`Blob/LastModified`|<span data-ttu-id="9ae1b-163">Data a času</span><span class="sxs-lookup"><span data-stu-id="9ae1b-163">DateTime</span></span>|<span data-ttu-id="9ae1b-164">Datum a čas poslední změny objektu blob, pro úlohu exportu.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-164">The date/time that the blob was last modified, for an export job only.</span></span>|  
|`Blob/ImportDisposition`|<span data-ttu-id="9ae1b-165">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9ae1b-165">String</span></span>|<span data-ttu-id="9ae1b-166">Import dispozice objektu blob pro úlohu importu.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-166">The import disposition of the blob, for an import job only.</span></span>|  
|`Blob/ImportDisposition/@Status`|<span data-ttu-id="9ae1b-167">Atribut, řetězec</span><span class="sxs-lookup"><span data-stu-id="9ae1b-167">Attribute, String</span></span>|<span data-ttu-id="9ae1b-168">Stav importu dispozice.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-168">The status of the import disposition.</span></span>|  
|`PageRangeList`|<span data-ttu-id="9ae1b-169">Vnořené – element XML</span><span class="sxs-lookup"><span data-stu-id="9ae1b-169">Nested XML element</span></span>|<span data-ttu-id="9ae1b-170">Představuje seznam rozsahů stránek pro objekt blob stránky.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-170">Represents a list of page ranges for a page blob.</span></span>|  
|`PageRange`|<span data-ttu-id="9ae1b-171">XML element</span><span class="sxs-lookup"><span data-stu-id="9ae1b-171">XML element</span></span>|<span data-ttu-id="9ae1b-172">Představuje rozsahu stránek.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-172">Represents a page range.</span></span>|  
|`PageRange/@Offset`|<span data-ttu-id="9ae1b-173">Atribut, celé číslo</span><span class="sxs-lookup"><span data-stu-id="9ae1b-173">Attribute, Integer</span></span>|<span data-ttu-id="9ae1b-174">Počáteční odsazení rozsahu stránek v objektu blob.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-174">Starting offset of the page range in the blob.</span></span>|  
|`PageRange/@Length`|<span data-ttu-id="9ae1b-175">Atribut, celé číslo</span><span class="sxs-lookup"><span data-stu-id="9ae1b-175">Attribute, Integer</span></span>|<span data-ttu-id="9ae1b-176">Délka v bajtech rozsahu stránek.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-176">Length in bytes of the page range.</span></span>|  
|`PageRange/@Hash`|<span data-ttu-id="9ae1b-177">Atribut, řetězec</span><span class="sxs-lookup"><span data-stu-id="9ae1b-177">Attribute, String</span></span>|<span data-ttu-id="9ae1b-178">Kódováním Base16 hodnotu hash MD5 rozsahu stránek.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-178">Base16-encoded MD5 hash of the page range.</span></span>|  
|`PageRange/@Status`|<span data-ttu-id="9ae1b-179">Atribut, řetězec</span><span class="sxs-lookup"><span data-stu-id="9ae1b-179">Attribute, String</span></span>|<span data-ttu-id="9ae1b-180">Stav zpracování rozsahu stránek.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-180">Status of processing the page range.</span></span>|  
|`BlockList`|<span data-ttu-id="9ae1b-181">Vnořené – element XML</span><span class="sxs-lookup"><span data-stu-id="9ae1b-181">Nested XML element</span></span>|<span data-ttu-id="9ae1b-182">Představuje seznam bloků pro objekt blob bloku.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-182">Represents a list of blocks for a block blob.</span></span>|  
|`Block`|<span data-ttu-id="9ae1b-183">XML element</span><span class="sxs-lookup"><span data-stu-id="9ae1b-183">XML element</span></span>|<span data-ttu-id="9ae1b-184">Představuje blok.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-184">Represents a block.</span></span>|  
|`Block/@Offset`|<span data-ttu-id="9ae1b-185">Atribut, celé číslo</span><span class="sxs-lookup"><span data-stu-id="9ae1b-185">Attribute, Integer</span></span>|<span data-ttu-id="9ae1b-186">Počáteční odsazení v objektu blob bloku.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-186">Starting offset of the block in the blob.</span></span>|  
|`Block/@Length`|<span data-ttu-id="9ae1b-187">Atribut, celé číslo</span><span class="sxs-lookup"><span data-stu-id="9ae1b-187">Attribute, Integer</span></span>|<span data-ttu-id="9ae1b-188">Délka v bajtech bloku.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-188">Length in bytes of the block.</span></span>|  
|`Block/@Id`|<span data-ttu-id="9ae1b-189">Atribut, řetězec</span><span class="sxs-lookup"><span data-stu-id="9ae1b-189">Attribute, String</span></span>|<span data-ttu-id="9ae1b-190">ID bloku.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-190">The block ID.</span></span>|  
|`Block/@Hash`|<span data-ttu-id="9ae1b-191">Atribut, řetězec</span><span class="sxs-lookup"><span data-stu-id="9ae1b-191">Attribute, String</span></span>|<span data-ttu-id="9ae1b-192">Kódováním Base16 hodnota hash MD5 bloku.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-192">Base16-encoded MD5 hash of the block.</span></span>|  
|`Block/@Status`|<span data-ttu-id="9ae1b-193">Atribut, řetězec</span><span class="sxs-lookup"><span data-stu-id="9ae1b-193">Attribute, String</span></span>|<span data-ttu-id="9ae1b-194">Stav zpracování bloku.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-194">Status of processing the block.</span></span>|  
|`Metadata`|<span data-ttu-id="9ae1b-195">Vnořené – element XML</span><span class="sxs-lookup"><span data-stu-id="9ae1b-195">Nested XML element</span></span>|<span data-ttu-id="9ae1b-196">Představuje metadata objektu blob.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-196">Represents the blob's metadata.</span></span>|  
|`Metadata/@Status`|<span data-ttu-id="9ae1b-197">Atribut, řetězec</span><span class="sxs-lookup"><span data-stu-id="9ae1b-197">Attribute, String</span></span>|<span data-ttu-id="9ae1b-198">Stav zpracování metadata objektu blob.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-198">Status of processing of the blob metadata.</span></span>|  
|`Metadata/GlobalPath`|<span data-ttu-id="9ae1b-199">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9ae1b-199">String</span></span>|<span data-ttu-id="9ae1b-200">Relativní cesta k souboru globální metadat.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-200">Relative path to the global metadata file.</span></span>|  
|`Metadata/GlobalPath/@Hash`|<span data-ttu-id="9ae1b-201">Atribut, řetězec</span><span class="sxs-lookup"><span data-stu-id="9ae1b-201">Attribute, String</span></span>|<span data-ttu-id="9ae1b-202">Kódováním Base16 hodnota hash MD5 globální metadata souboru.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-202">Base16-encoded MD5 hash of the global metadata file.</span></span>|  
|`Metadata/Path`|<span data-ttu-id="9ae1b-203">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9ae1b-203">String</span></span>|<span data-ttu-id="9ae1b-204">Relativní cesta k souboru metadat.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-204">Relative path to the metadata file.</span></span>|  
|`Metadata/Path/@Hash`|<span data-ttu-id="9ae1b-205">Atribut, řetězec</span><span class="sxs-lookup"><span data-stu-id="9ae1b-205">Attribute, String</span></span>|<span data-ttu-id="9ae1b-206">Kódováním Base16 hodnota hash MD5 souboru metadat.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-206">Base16-encoded MD5 hash of the metadata file.</span></span>|  
|`Properties`|<span data-ttu-id="9ae1b-207">Vnořené – element XML</span><span class="sxs-lookup"><span data-stu-id="9ae1b-207">Nested XML element</span></span>|<span data-ttu-id="9ae1b-208">Představuje vlastnosti objektů blob.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-208">Represents the blob properties.</span></span>|  
|`Properties/@Status`|<span data-ttu-id="9ae1b-209">Atribut, řetězec</span><span class="sxs-lookup"><span data-stu-id="9ae1b-209">Attribute, String</span></span>|<span data-ttu-id="9ae1b-210">Stav zpracování objektu blob vlastnosti, například soubor nebyl nalezen, byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-210">Status of processing the blob properties, e.g. file not found, completed.</span></span>|  
|`Properties/GlobalPath`|<span data-ttu-id="9ae1b-211">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9ae1b-211">String</span></span>|<span data-ttu-id="9ae1b-212">Relativní cesta k souboru globální vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-212">Relative path to the global properties file.</span></span>|  
|`Properties/GlobalPath/@Hash`|<span data-ttu-id="9ae1b-213">Atribut, řetězec</span><span class="sxs-lookup"><span data-stu-id="9ae1b-213">Attribute, String</span></span>|<span data-ttu-id="9ae1b-214">Kódováním Base16 hodnota hash MD5 globální vlastnosti souboru.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-214">Base16-encoded MD5 hash of the global properties file.</span></span>|  
|`Properties/Path`|<span data-ttu-id="9ae1b-215">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9ae1b-215">String</span></span>|<span data-ttu-id="9ae1b-216">Relativní cesta k souboru vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-216">Relative path to the properties file.</span></span>|  
|`Properties/Path/@Hash`|<span data-ttu-id="9ae1b-217">Atribut, řetězec</span><span class="sxs-lookup"><span data-stu-id="9ae1b-217">Attribute, String</span></span>|<span data-ttu-id="9ae1b-218">Kódováním Base16 hodnota hash MD5 souboru vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-218">Base16-encoded MD5 hash of the properties file.</span></span>|  
|`Blob/Status`|<span data-ttu-id="9ae1b-219">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9ae1b-219">String</span></span>|<span data-ttu-id="9ae1b-220">Stav zpracování objektu blob.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-220">Status of processing the blob.</span></span>|  
  
# <a name="drive-status-codes"></a><span data-ttu-id="9ae1b-221">Jednotka stavové kódy</span><span class="sxs-lookup"><span data-stu-id="9ae1b-221">Drive status codes</span></span>  
<span data-ttu-id="9ae1b-222">Následující tabulka uvádí stavové kódy pro zpracování na jednotku.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-222">The following table lists the status codes for processing a drive.</span></span>  
  
|<span data-ttu-id="9ae1b-223">Stavový kód</span><span class="sxs-lookup"><span data-stu-id="9ae1b-223">Status code</span></span>|<span data-ttu-id="9ae1b-224">Popis</span><span class="sxs-lookup"><span data-stu-id="9ae1b-224">Description</span></span>|  
|-----------------|-----------------|  
|`Completed`|<span data-ttu-id="9ae1b-225">Jednotka dokončil zpracování bez chyb.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-225">The drive has finished processing without any errors.</span></span>|  
|`CompletedWithWarnings`|<span data-ttu-id="9ae1b-226">Jednotka dokončení zpracování se upozornění v jedné nebo více objektů blob na import potížemi, zadaná pro objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-226">The drive has finished processing with warnings in one or more blobs per the import dispositions specified for the blobs.</span></span>|  
|`CompletedWithErrors`|<span data-ttu-id="9ae1b-227">Jednotka bylo dokončeno s chybami v jedné nebo více objektů BLOB nebo bloků.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-227">The drive has finished with errors in one or more blobs or chunks.</span></span>|  
|`DiskNotFound`|<span data-ttu-id="9ae1b-228">Žádný disk nachází na jednotce.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-228">No disk is found on the drive.</span></span>|  
|`VolumeNotNtfs`|<span data-ttu-id="9ae1b-229">První datový svazek na disku není ve formátu systému souborů NTFS.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-229">The first data volume on the disk is not in NTFS format.</span></span>|  
|`DiskOperationFailed`|<span data-ttu-id="9ae1b-230">Při provádění operací na jednotce došlo k neznámé chybě.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-230">An unknown failure occurred when performing operations on the drive.</span></span>|  
|`BitLockerVolumeNotFound`|<span data-ttu-id="9ae1b-231">Nebyla nalezena žádná encryptable svazku Bitlockeru.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-231">No BitLocker encryptable volume is found.</span></span>|  
|`BitLockerNotActivated`|<span data-ttu-id="9ae1b-232">Nástroj BitLocker není povolen ve svazku.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-232">BitLocker is not enabled on the volume.</span></span>|  
|`BitLockerProtectorNotFound`|<span data-ttu-id="9ae1b-233">Ochranné zařízení klíče číselné heslo neexistuje na svazku.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-233">The numerical password key protector does not exist on the volume.</span></span>|  
|`BitLockerKeyInvalid`|<span data-ttu-id="9ae1b-234">Číselné heslo zadané nelze svazek odemknout.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-234">The numerical password provided cannot unlock the volume.</span></span>|  
|`BitLockerUnlockVolumeFailed`|<span data-ttu-id="9ae1b-235">Došlo k neznámé chybě došlo při pokusu o svazek odemknout.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-235">Unknown failure has happened when trying to unlock the volume.</span></span>|  
|`BitLockerFailed`|<span data-ttu-id="9ae1b-236">Při provádění operací se BitLocker došlo k neznámé chybě.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-236">An unknown failure occurred while performing BitLocker operations.</span></span>|  
|`ManifestNameInvalid`|<span data-ttu-id="9ae1b-237">Název souboru manifestu je neplatný.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-237">The manifest file name is invalid.</span></span>|  
|`ManifestNameTooLong`|<span data-ttu-id="9ae1b-238">Název souboru manifestu je příliš dlouhý.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-238">The manifest file name is too long.</span></span>|  
|`ManifestNotFound`|<span data-ttu-id="9ae1b-239">Soubor manifestu nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-239">The manifest file is not found.</span></span>|  
|`ManifestAccessDenied`|<span data-ttu-id="9ae1b-240">Přístup k souboru manifestu byl odepřen.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-240">Access to the manifest file is denied.</span></span>|  
|`ManifestCorrupted`|<span data-ttu-id="9ae1b-241">Je poškozený soubor manifestu (obsah neodpovídá jeho hash).</span><span class="sxs-lookup"><span data-stu-id="9ae1b-241">The manifest file is corrupted (the content does not match its hash).</span></span>|  
|`ManifestFormatInvalid`|<span data-ttu-id="9ae1b-242">Obsah manifestu neodpovídá požadovaný formát.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-242">The manifest content does not conform to the required format.</span></span>|  
|`ManifestDriveIdMismatch`|<span data-ttu-id="9ae1b-243">ID disku v souboru manifestu neodpovídá jeden pro čtení z disku.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-243">The drive ID in the manifest file does not match the one read from the drive.</span></span>|  
|`ReadManifestFailed`|<span data-ttu-id="9ae1b-244">Při čtení z manifestu došlo k chybě vstupně-výstupních operací disku.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-244">A disk I/O failure occurred while reading from the manifest.</span></span>|  
|`BlobListFormatInvalid`|<span data-ttu-id="9ae1b-245">Objekt blob seznamu objektů blob export neodpovídá požadovaný formát.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-245">The export blob list blob does not conform to the required format.</span></span>|  
|`BlobRequestForbidden`|<span data-ttu-id="9ae1b-246">Přístup k objektům BLOB v účtu úložiště je zakázán.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-246">Access to the blobs in the storage account is forbidden.</span></span> <span data-ttu-id="9ae1b-247">Důvodem může být klíč účtu úložiště nejsou platné nebo sdíleného přístupového podpisu kontejneru.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-247">This might be due to invalid storage account key or container SAS.</span></span>|  
|`InternalError`|<span data-ttu-id="9ae1b-248">A při zpracování jednotce došlo k vnitřní chybě.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-248">And internal error occurred while processing the drive.</span></span>|  
  
## <a name="blob-status-codes"></a><span data-ttu-id="9ae1b-249">Objekt BLOB stavové kódy</span><span class="sxs-lookup"><span data-stu-id="9ae1b-249">Blob status codes</span></span>  
<span data-ttu-id="9ae1b-250">Následující tabulka uvádí stavové kódy pro zpracování objektu blob.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-250">The following table lists the status codes for processing a blob.</span></span>  
  
|<span data-ttu-id="9ae1b-251">Stavový kód</span><span class="sxs-lookup"><span data-stu-id="9ae1b-251">Status code</span></span>|<span data-ttu-id="9ae1b-252">Popis</span><span class="sxs-lookup"><span data-stu-id="9ae1b-252">Description</span></span>|  
|-----------------|-----------------|  
|`Completed`|<span data-ttu-id="9ae1b-253">Objekt blob se dokončil zpracování bez chyb.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-253">The blob has finished processing without errors.</span></span>|  
|`CompletedWithErrors`|<span data-ttu-id="9ae1b-254">Objekt blob se dokončil zpracování s chybami v jedné nebo více rozsahů stránek nebo bloky, metadata nebo vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-254">The blob has finished processing with errors in one or more page ranges or blocks, metadata, or properties.</span></span>|  
|`FileNameInvalid`|<span data-ttu-id="9ae1b-255">Název souboru je neplatný.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-255">The file name is invalid.</span></span>|  
|`FileNameTooLong`|<span data-ttu-id="9ae1b-256">Název souboru je příliš dlouhý.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-256">The file name is too long.</span></span>|  
|`FileNotFound`|<span data-ttu-id="9ae1b-257">Soubor nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-257">The file is not found.</span></span>|  
|`FileAccessDenied`|<span data-ttu-id="9ae1b-258">Přístup k souboru byl odepřen.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-258">Access to the file is denied.</span></span>|  
|`BlobRequestFailed`|<span data-ttu-id="9ae1b-259">Žádost o služby objektů Blob pro přístup k objektu blob se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-259">The Blob service request to access the blob has failed.</span></span>|  
|`BlobRequestForbidden`|<span data-ttu-id="9ae1b-260">Žádost o služby objektů Blob pro přístup k objektu blob je zakázáno.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-260">The Blob service request to access the blob is forbidden.</span></span> <span data-ttu-id="9ae1b-261">Důvodem může být klíč účtu úložiště nejsou platné nebo sdíleného přístupového podpisu kontejneru.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-261">This might be due to invalid storage account key or container SAS.</span></span>|  
|`RenameFailed`|<span data-ttu-id="9ae1b-262">Přejmenujte soubor (pro úlohy exportu) nebo objektu blob (pro úlohy importu) se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-262">Failed to rename the blob (for an import job) or the file (for an export job).</span></span>|  
|`BlobUnexpectedChange`|<span data-ttu-id="9ae1b-263">S objektem blob (pro úlohy exportu) došlo k neočekávané chování.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-263">An unexpected change has occurred with the blob (for an export job).</span></span>|  
|`LeasePresent`|<span data-ttu-id="9ae1b-264">Není přítomen v objektu blob zapůjčení.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-264">There is a lease present on the blob.</span></span>|  
|`IOFailed`|<span data-ttu-id="9ae1b-265">Při zpracování objektu blob došlo k chybě vstupně-výstupních operací disku nebo v síti.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-265">A disk or network I/O failure occurred while processing the blob.</span></span>|  
|`Failed`|<span data-ttu-id="9ae1b-266">Při zpracování objektu blob došlo k neznámé chybě.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-266">An unknown failure occurred while processing the blob.</span></span>|  
  
## <a name="import-disposition-status-codes"></a><span data-ttu-id="9ae1b-267">Import dispozice stavové kódy</span><span class="sxs-lookup"><span data-stu-id="9ae1b-267">Import disposition status codes</span></span>  
<span data-ttu-id="9ae1b-268">Následující tabulka uvádí stavové kódy pro řešení importu dispozice.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-268">The following table lists the status codes for resolving an import disposition.</span></span>  
  
|<span data-ttu-id="9ae1b-269">Stavový kód</span><span class="sxs-lookup"><span data-stu-id="9ae1b-269">Status code</span></span>|<span data-ttu-id="9ae1b-270">Popis</span><span class="sxs-lookup"><span data-stu-id="9ae1b-270">Description</span></span>|  
|-----------------|-----------------|  
|`Created`|<span data-ttu-id="9ae1b-271">Vytvořil se objekt blob.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-271">The blob has been created.</span></span>|  
|`Renamed`|<span data-ttu-id="9ae1b-272">Objekt blob byl přejmenován na přejmenování import dispozice.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-272">The blob has been renamed per rename import disposition.</span></span> <span data-ttu-id="9ae1b-273">`Blob/BlobPath` Element obsahuje identifikátor URI objektu blob přejmenovat.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-273">The `Blob/BlobPath` element contains the URI for the renamed blob.</span></span>|  
|`Skipped`|<span data-ttu-id="9ae1b-274">Objekt blob byl vynechán za `no-overwrite` importovat dispozice.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-274">The blob has been skipped per `no-overwrite` import disposition.</span></span>|  
|`Overwritten`|<span data-ttu-id="9ae1b-275">Objekt blob má přepsat existující objekt blob za `overwrite` importovat dispozice.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-275">The blob has overwritten an existing blob per `overwrite` import disposition.</span></span>|  
|`Cancelled`|<span data-ttu-id="9ae1b-276">Předchozí selhání byla zastavena, další zpracování import dispozice.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-276">A prior failure has stopped further processing of the import disposition.</span></span>|  
  
## <a name="page-rangeblock-status-codes"></a><span data-ttu-id="9ae1b-277">Stránka rozsah/blok stavové kódy</span><span class="sxs-lookup"><span data-stu-id="9ae1b-277">Page range/block status codes</span></span>  
<span data-ttu-id="9ae1b-278">Následující tabulka uvádí stavové kódy pro zpracování stránky rozsah nebo blok.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-278">The following table lists the status codes for processing a page range or a block.</span></span>  
  
|<span data-ttu-id="9ae1b-279">Stavový kód</span><span class="sxs-lookup"><span data-stu-id="9ae1b-279">Status code</span></span>|<span data-ttu-id="9ae1b-280">Popis</span><span class="sxs-lookup"><span data-stu-id="9ae1b-280">Description</span></span>|  
|-----------------|-----------------|  
|`Completed`|<span data-ttu-id="9ae1b-281">Stránka rozsah nebo blok dokončení zpracování bez chyb.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-281">The page range or block has finished processing without any errors.</span></span>|  
|`Committed`|<span data-ttu-id="9ae1b-282">Blok potvrzeny, ale v bloku úplný seznam protože jiné bloky se nezdařily, nebo put samotný seznam úplné bloku nedošlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-282">The block has been committed,  but not in the full block list because other blocks have failed, or put full block list itself has failed.</span></span>|  
|`Uncommitted`|<span data-ttu-id="9ae1b-283">Blok je odeslat, ale nikoli potvrdit.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-283">The block is uploaded but not committed.</span></span>|  
|`Corrupted`|<span data-ttu-id="9ae1b-284">Stránka rozsah nebo blok je poškozený (obsah neodpovídá jeho hash).</span><span class="sxs-lookup"><span data-stu-id="9ae1b-284">The page range or block is corrupted (the content does not match its hash).</span></span>|  
|`FileUnexpectedEnd`|<span data-ttu-id="9ae1b-285">Byl nalezen neočekávaný konec souboru.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-285">An unexpected end of file has been encountered.</span></span>|  
|`BlobUnexpectedEnd`|<span data-ttu-id="9ae1b-286">Byl nalezen neočekávaný konec objektu blob.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-286">An unexpected end of blob has been encountered.</span></span>|  
|`BlobRequestFailed`|<span data-ttu-id="9ae1b-287">Žádost o služby objektů Blob pro přístup k rozsahu stránek nebo bloku selhal.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-287">The Blob service request to access the page range or block has failed.</span></span>|  
|`IOFailed`|<span data-ttu-id="9ae1b-288">Během zpracování stránky rozsah nebo blok došlo k chybě vstupně-výstupních operací disku nebo v síti.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-288">A disk or network I/O failure occurred while processing the page range or block.</span></span>|  
|`Failed`|<span data-ttu-id="9ae1b-289">Během zpracování stránky rozsah nebo blok došlo k neznámé chybě.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-289">An unknown failure occurred while processing the page range or block.</span></span>|  
|`Cancelled`|<span data-ttu-id="9ae1b-290">Předchozí selhání byla zastavena, další zpracování rozsahu stránek nebo bloku.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-290">A prior failure has stopped further processing of the page range or block.</span></span>|  
  
## <a name="metadata-status-codes"></a><span data-ttu-id="9ae1b-291">Metadata stavové kódy</span><span class="sxs-lookup"><span data-stu-id="9ae1b-291">Metadata status codes</span></span>  
<span data-ttu-id="9ae1b-292">Následující tabulka uvádí stavové kódy pro zpracování metadata objektu blob.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-292">The following table lists the status codes for processing blob metadata.</span></span>  
  
|<span data-ttu-id="9ae1b-293">Stavový kód</span><span class="sxs-lookup"><span data-stu-id="9ae1b-293">Status code</span></span>|<span data-ttu-id="9ae1b-294">Popis</span><span class="sxs-lookup"><span data-stu-id="9ae1b-294">Description</span></span>|  
|-----------------|-----------------|  
|`Completed`|<span data-ttu-id="9ae1b-295">Metadata se dokončil zpracování bez chyb.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-295">The metadata has finished processing without errors.</span></span>|  
|`FileNameInvalid`|<span data-ttu-id="9ae1b-296">Název souboru metadat je neplatná.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-296">The metadata file name is invalid.</span></span>|  
|`FileNameTooLong`|<span data-ttu-id="9ae1b-297">Název souboru metadat je příliš dlouhý.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-297">The metadata file name is too long.</span></span>|  
|`FileNotFound`|<span data-ttu-id="9ae1b-298">Soubor metadat nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-298">The metadata file is not found.</span></span>|  
|`FileAccessDenied`|<span data-ttu-id="9ae1b-299">Přístup k souboru metadat byl odepřen.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-299">Access to the metadata file is denied.</span></span>|  
|`Corrupted`|<span data-ttu-id="9ae1b-300">Je poškozený soubor metadat (obsah neodpovídá jeho hash).</span><span class="sxs-lookup"><span data-stu-id="9ae1b-300">The metadata file is corrupted (the content does not match its hash).</span></span>|  
|`XmlReadFailed`|<span data-ttu-id="9ae1b-301">Metadata obsahu neodpovídá požadovaný formát.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-301">The metadata content does not conform to the required format.</span></span>|  
|`XmlWriteFailed`|<span data-ttu-id="9ae1b-302">Zápis metadata XML se nezdařila.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-302">Writing the metadata XML has failed.</span></span>|  
|`BlobRequestFailed`|<span data-ttu-id="9ae1b-303">Žádost o služby objektů Blob přístup k metadatům se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-303">The Blob service request to access the metadata has failed.</span></span>|  
|`IOFailed`|<span data-ttu-id="9ae1b-304">Při zpracování metadat došlo k chybě vstupně-výstupních operací disku nebo v síti.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-304">A disk or network I/O failure occurred while processing the metadata.</span></span>|  
|`Failed`|<span data-ttu-id="9ae1b-305">Při zpracování metadat došlo k neznámé chybě.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-305">An unknown failure occurred while processing the metadata.</span></span>|  
|`Cancelled`|<span data-ttu-id="9ae1b-306">Předchozí selhání byla zastavena, další zpracování metadat.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-306">A prior failure has stopped further processing of the metadata.</span></span>|  
  
## <a name="properties-status-codes"></a><span data-ttu-id="9ae1b-307">Vlastnosti stavové kódy</span><span class="sxs-lookup"><span data-stu-id="9ae1b-307">Properties status codes</span></span>  
<span data-ttu-id="9ae1b-308">Následující tabulka uvádí stavové kódy pro zpracování vlastnosti objektů blob.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-308">The following table lists the status codes for processing blob properties.</span></span>  
  
|<span data-ttu-id="9ae1b-309">Stavový kód</span><span class="sxs-lookup"><span data-stu-id="9ae1b-309">Status code</span></span>|<span data-ttu-id="9ae1b-310">Popis</span><span class="sxs-lookup"><span data-stu-id="9ae1b-310">Description</span></span>|  
|-----------------|-----------------|  
|`Completed`|<span data-ttu-id="9ae1b-311">Vlastnosti dokončení zpracování bez chyb.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-311">The properties have finished processing without any errors.</span></span>|  
|`FileNameInvalid`|<span data-ttu-id="9ae1b-312">Název vlastnosti souboru je neplatný.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-312">The properties file name is invalid.</span></span>|  
|`FileNameTooLong`|<span data-ttu-id="9ae1b-313">Název vlastnosti souboru je příliš dlouhý.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-313">The properties file name is too long.</span></span>|  
|`FileNotFound`|<span data-ttu-id="9ae1b-314">Vlastnosti soubor nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-314">The properties file is not found.</span></span>|  
|`FileAccessDenied`|<span data-ttu-id="9ae1b-315">Byl odepřen přístup k vlastnosti souboru.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-315">Access to the properties file is denied.</span></span>|  
|`Corrupted`|<span data-ttu-id="9ae1b-316">Vlastnosti soubor je poškozený (obsah neodpovídá jeho hash).</span><span class="sxs-lookup"><span data-stu-id="9ae1b-316">The properties file is corrupted (the content does not match its hash).</span></span>|  
|`XmlReadFailed`|<span data-ttu-id="9ae1b-317">Vlastnosti obsahu neodpovídá požadovaný formát.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-317">The properties content does not conform to the required format.</span></span>|  
|`XmlWriteFailed`|<span data-ttu-id="9ae1b-318">Zápis vlastností XML se nezdařila.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-318">Writing the properties XML has failed.</span></span>|  
|`BlobRequestFailed`|<span data-ttu-id="9ae1b-319">Žádost o služby objektů Blob pro přístup k vlastnostem se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-319">The Blob service request to access the properties has failed.</span></span>|  
|`IOFailed`|<span data-ttu-id="9ae1b-320">Při zpracování vlastnosti došlo k chybě vstupně-výstupních operací disku nebo v síti.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-320">A disk or network I/O failure occurred while processing the properties.</span></span>|  
|`Failed`|<span data-ttu-id="9ae1b-321">Při zpracování vlastnosti došlo k neznámé chybě.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-321">An unknown failure occurred while processing the properties.</span></span>|  
|`Cancelled`|<span data-ttu-id="9ae1b-322">Předchozí selhání byla zastavena, další zpracování vlastností.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-322">A prior failure has stopped further processing of the properties.</span></span>|  
  
## <a name="sample-logs"></a><span data-ttu-id="9ae1b-323">Ukázka protokoly</span><span class="sxs-lookup"><span data-stu-id="9ae1b-323">Sample logs</span></span>  
<span data-ttu-id="9ae1b-324">Následuje příklad podrobného protokolování.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-324">The following is an example of verbose log.</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<DriveLog Version="2014-11-01">  
    <DriveId>WD-WMATV123456</DriveId>  
    <Blob Status="Completed">  
       <BlobPath>pictures/bob/wild/desert.jpg</BlobPath>  
       <FilePath>\Users\bob\Pictures\wild\desert.jpg</FilePath>  
       <Length>98304</Length>  
       <ImportDisposition Status="Created">overwrite</ImportDisposition>  
       <BlockList>  
          <Block Offset="0" Length="65536" Id="AAAAAA==" Hash=" 9C8AE14A55241F98533C4D80D85CDC68" Status="Completed"/>  
          <Block Offset="65536" Length="32768" Id="AQAAAA==" Hash=" DF54C531C9B3CA2570FDDDB3BCD0E27D" Status="Completed"/>  
       </BlockList>  
       <Metadata Status="Completed">  
          <GlobalPath Hash=" E34F54B7086BCF4EC1601D056F4C7E37">\Users\bob\Pictures\wild\metadata.xml</GlobalPath>  
       </Metadata>  
    </Blob>  
    <Blob Status="CompletedWithErrors">  
       <BlobPath>pictures/bob/animals/koala.jpg</BlobPath>  
       <FilePath>\Users\bob\Pictures\animals\koala.jpg</FilePath>  
       <Length>163840</Length>  
       <ImportDisposition Status="Overwritten">overwrite</ImportDisposition>  
       <PageRangeList>  
          <PageRange Offset="0" Length="65536" Hash="19701B8877418393CB3CB567F53EE225" Status="Completed"/>  
          <PageRange Offset="65536" Length="65536" Hash="AA2585F6F6FD01C4AD4256E018240CD4" Status="Corrupted"/>  
          <PageRange Offset="131072" Length="4096" Hash="9BA552E1C3EEAFFC91B42B979900A996" Status="Completed"/>  
       </PageRangeList>  
       <Properties Status="Completed">  
          <Path Hash="38D7AE80653F47F63C0222FEE90EC4E7">\Users\bob\Pictures\animals\koala.jpg.properties</Path>  
       </Properties>  
    </Blob>  
    <Status>CompletedWithErrors</Status>  
</DriveLog>  
```  
  
<span data-ttu-id="9ae1b-325">Odpovídající protokolu chyb jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-325">The corresponding error log is shown below.</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<DriveLog Version="2014-11-01">  
    <DriveId>WD-WMATV6965824</DriveId>  
    <Blob Status="CompletedWithErrors">  
       <BlobPath>pictures/bob/animals/koala.jpg</BlobPath>  
       <FilePath>\Users\bob\Pictures\animals\koala.jpg</FilePath>  
       <Length>163840</Length>  
       <ImportDisposition Status="Overwritten">overwrite</ImportDisposition>  
       <PageRangeList>  
          <PageRange Offset="65536" Length="65536" Hash="AA2585F6F6FD01C4AD4256E018240CD4" Status="Corrupted"/>  
       </PageRangeList>  
    </Blob>  
    <Status>CompletedWithErrors</Status>  
</DriveLog>  
```

 <span data-ttu-id="9ae1b-326">Úlohy importu v protokolu chyb postupujte podle obsahuje chybu o soubor nebyl nalezen na disku pro import.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-326">The follow error log for an import job contains an error about a file not found on the import drive.</span></span> <span data-ttu-id="9ae1b-327">Všimněte si, že je stav následující součásti `Cancelled`.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-327">Note that the status of subsequent components is `Cancelled`.</span></span>  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveLog Version="2014-11-01">  
  <DriveId>9WM35C2V</DriveId>  
  <Blob Status="FileNotFound">  
    <BlobPath>pictures/animals/koala.jpg</BlobPath>  
    <FilePath>\animals\koala.jpg</FilePath>  
    <Length>30310</Length>  
    <ImportDisposition Status="Cancelled">rename</ImportDisposition>  
    <BlockList>  
      <Block Offset="0" Length="6062" Id="MD5/cAzn4h7VVSWXf696qp5Uaw==" Hash="700CE7E21ED55525977FAF7AAA9E546B" Status="Cancelled" />  
      <Block Offset="6062" Length="6062" Id="MD5/PEnGwYOI8LPLNYdfKr7kAg==" Hash="3C49C6C18388F0B3CB35875F2ABEE402" Status="Cancelled" />  
      <Block Offset="12124" Length="6062" Id="MD5/FG4WxqfZKuUWZ2nGTU2qVA==" Hash="146E16C6A7D92AE5166769C64D4DAA54" Status="Cancelled" />  
      <Block Offset="18186" Length="6062" Id="MD5/ZzibNDzr3IRBQENRyegeXQ==" Hash="67389B343CEBDC8441404351C9E81E5D" Status="Cancelled" />  
      <Block Offset="24248" Length="6062" Id="MD5/ZzibNDzr3IRBQENRyegeXQ==" Hash="67389B343CEBDC8441404351C9E81E5D" Status="Cancelled" />  
    </BlockList>  
  </Blob>  
  <Status>CompletedWithErrors</Status>  
</DriveLog>  
```

<span data-ttu-id="9ae1b-328">Následující v protokolu chyb úlohy exportu označuje, že obsah objektu blob byla úspěšně zapsána na jednotku, ale, že došlo k chybě při exportu vlastnosti objektu blob.</span><span class="sxs-lookup"><span data-stu-id="9ae1b-328">The following error log for an export job indicates that the blob content has been successfully written to the drive, but that an error occurred while exporting the blob's properties.</span></span>  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveLog Version="2014-11-01">  
  <DriveId>9WM35C3U</DriveId>  
  <Blob Status="CompletedWithErrors">  
    <BlobPath>pictures/wild/canyon.jpg</BlobPath>  
    <FilePath>\pictures\wild\canyon.jpg</FilePath>  
    <LastModified>2012-09-18T23:47:08Z</LastModified>  
    <Length>163840</Length>  
    <BlockList />  
    <Properties Status="Failed" />  
  </Blob>  
  <Status>CompletedWithErrors</Status>  
</DriveLog>  
```
  
## <a name="next-steps"></a><span data-ttu-id="9ae1b-329">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9ae1b-329">Next steps</span></span>
 
* [<span data-ttu-id="9ae1b-330">REST API pro Import/Export úložiště</span><span class="sxs-lookup"><span data-stu-id="9ae1b-330">Storage Import/Export REST API</span></span>](/rest/api/storageimportexport/)
