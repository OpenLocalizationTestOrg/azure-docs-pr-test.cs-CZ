---
title: "Formát souboru protokolu importu a exportu aaaAzure | Microsoft Docs"
description: "Další informace o hello formát souborů protokolu hello vytvoří při provádění kroků pro úlohu importu/exportu služby."
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
ms.openlocfilehash: 15a652455aa947922af0aa39ccefe68811a3db19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-importexport-service-log-file-format"></a><span data-ttu-id="fd296-103">Formát souboru protokolu služby sady Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="fd296-103">Azure Import/Export service log file format</span></span>
<span data-ttu-id="fd296-104">Když hello služby Microsoft Azure Import/Export provede akci na disku jako součást úlohy importu nebo úlohy exportu, protokoly zapisují tooblock objekty BLOB v účtu úložiště hello přidruženého k této úlohy.</span><span class="sxs-lookup"><span data-stu-id="fd296-104">When hello Microsoft Azure Import/Export service performs an action on a drive as part of an import job or an export job, logs are written tooblock blobs in hello storage account associated with that job.</span></span>  
  
<span data-ttu-id="fd296-105">Existují dva protokoly, které může být naprogramovaný hello službu Import/Export:</span><span class="sxs-lookup"><span data-stu-id="fd296-105">There are two logs that may be written by hello Import/Export service:</span></span>  
  
-   <span data-ttu-id="fd296-106">v případě hello chyby vždy generování Hello protokolu chyb.</span><span class="sxs-lookup"><span data-stu-id="fd296-106">hello error log is always generated in hello event of an error.</span></span>  
  
-   <span data-ttu-id="fd296-107">Hello podrobného protokolování není ve výchozím nastavení povolené, ale lze ho zapnout pomocí nastavení hello `EnableVerboseLog` vlastnost [Put úlohy](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) nebo [vlastnosti úlohy aktualizace](/rest/api/storageimportexport/jobs#Jobs_Update) operaci.</span><span class="sxs-lookup"><span data-stu-id="fd296-107">hello verbose log is not enabled by default, but may be enabled by setting hello `EnableVerboseLog` property on a [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) or [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) operation.</span></span>  
  
## <a name="log-file-location"></a><span data-ttu-id="fd296-108">Umístění souboru protokolu</span><span class="sxs-lookup"><span data-stu-id="fd296-108">Log file location</span></span>  
<span data-ttu-id="fd296-109">Hello se protokoly zapisují tooblock objekty BLOB v kontejneru hello nebo virtuální adresář zadaný hello `ImportExportStatesPath` nastavení, které můžete nastavit `Put Job` operaci.</span><span class="sxs-lookup"><span data-stu-id="fd296-109">hello logs are written tooblock blobs in hello container or virtual directory specified by hello `ImportExportStatesPath` setting, which you can set on a `Put Job` operation.</span></span> <span data-ttu-id="fd296-110">Hello umístění toowhich hello se protokoly zapisují závisí na tom, jak je zadán ověřování pro úlohu hello, společně s hello hodnota zadaná pro `ImportExportStatesPath`.</span><span class="sxs-lookup"><span data-stu-id="fd296-110">hello location toowhich hello logs are written depends on how authentication is specified for hello job, together with hello value specified for `ImportExportStatesPath`.</span></span> <span data-ttu-id="fd296-111">Ověřování pro úlohu hello je možné zadat pomocí klíč účtu úložiště nebo kontejneru SAS (sdílený přístupový podpis).</span><span class="sxs-lookup"><span data-stu-id="fd296-111">Authentication for hello job may be specified via a storage account key, or a container SAS (shared access signature).</span></span>  
  
<span data-ttu-id="fd296-112">Hello název kontejneru hello nebo virtuální adresář může být hello výchozí název `waimportexport`, nebo jiný kontejner nebo název virtuálního adresáře, který určíte.</span><span class="sxs-lookup"><span data-stu-id="fd296-112">hello name of hello container or virtual directory may either be hello default name of `waimportexport`, or another container or virtual directory name that you specify.</span></span>  
  
<span data-ttu-id="fd296-113">Následující tabulka Hello ukazuje hello způsobů:</span><span class="sxs-lookup"><span data-stu-id="fd296-113">hello table below shows hello possible options:</span></span>  
  
|<span data-ttu-id="fd296-114">Metoda ověřování</span><span class="sxs-lookup"><span data-stu-id="fd296-114">Authentication Method</span></span>|<span data-ttu-id="fd296-115">Hodnota `ImportExportStatesPath`– Element</span><span class="sxs-lookup"><span data-stu-id="fd296-115">Value of `ImportExportStatesPath`Element</span></span>|<span data-ttu-id="fd296-116">Umístění protokolu objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="fd296-116">Location of Log Blobs</span></span>|  
|---------------------------|----------------------------------------------|---------------------------|  
|<span data-ttu-id="fd296-117">Klíče účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="fd296-117">Storage account key</span></span>|<span data-ttu-id="fd296-118">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="fd296-118">Default value</span></span>|<span data-ttu-id="fd296-119">Kontejner s názvem `waimportexport`, což je výchozí kontejner hello.</span><span class="sxs-lookup"><span data-stu-id="fd296-119">A container named `waimportexport`, which is hello default container.</span></span> <span data-ttu-id="fd296-120">Například:</span><span class="sxs-lookup"><span data-stu-id="fd296-120">For example:</span></span><br /><br /> `https://myaccount.blob.core.windows.net/waimportexport`|  
|<span data-ttu-id="fd296-121">Klíče účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="fd296-121">Storage account key</span></span>|<span data-ttu-id="fd296-122">Uživatelem zadanou hodnotu</span><span class="sxs-lookup"><span data-stu-id="fd296-122">User-specified value</span></span>|<span data-ttu-id="fd296-123">Kontejner s názvem uživatelem hello.</span><span class="sxs-lookup"><span data-stu-id="fd296-123">A container named by hello user.</span></span> <span data-ttu-id="fd296-124">Například:</span><span class="sxs-lookup"><span data-stu-id="fd296-124">For example:</span></span><br /><br /> `https://myaccount.blob.core.windows.net/mylogcontainer`|  
|<span data-ttu-id="fd296-125">Sdíleného přístupového podpisu kontejneru</span><span class="sxs-lookup"><span data-stu-id="fd296-125">Container SAS</span></span>|<span data-ttu-id="fd296-126">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="fd296-126">Default value</span></span>|<span data-ttu-id="fd296-127">Virtuální adresář s názvem `waimportexport`, což je hello výchozí název, pod kontejnerem hello zadaný v hello SAS.</span><span class="sxs-lookup"><span data-stu-id="fd296-127">A virtual directory named `waimportexport`, which is hello default name, beneath hello container specified in hello SAS.</span></span><br /><br /> <span data-ttu-id="fd296-128">Například pokud hello SAS zadaná pro úlohu hello je `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, pak by byl umístění protokolu hello`https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport`</span><span class="sxs-lookup"><span data-stu-id="fd296-128">For example, if hello SAS specified for hello job is  `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, then hello log location would be `https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport`</span></span>|  
|<span data-ttu-id="fd296-129">Sdíleného přístupového podpisu kontejneru</span><span class="sxs-lookup"><span data-stu-id="fd296-129">Container SAS</span></span>|<span data-ttu-id="fd296-130">Uživatelem zadanou hodnotu</span><span class="sxs-lookup"><span data-stu-id="fd296-130">User-specified value</span></span>|<span data-ttu-id="fd296-131">Virtuální adresář s názvem uživatelem hello pod kontejnerem hello zadaný v hello SAS.</span><span class="sxs-lookup"><span data-stu-id="fd296-131">A virtual directory named by hello user, beneath hello container specified in hello SAS.</span></span><br /><br /> <span data-ttu-id="fd296-132">Například pokud hello SAS zadaná pro úlohu hello je `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, a hello zadaný virtuální adresář název `mylogblobs`, pak by byl umístění protokolu hello `https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport/mylogblobs`.</span><span class="sxs-lookup"><span data-stu-id="fd296-132">For example, if hello SAS specified for hello job is  `https://myaccount.blob.core.windows.net/mylogcontainer?sv=2012-02-12&se=2015-05-22T06%3A54%3A55Z&sr=c&sp=wl&sig=sigvalue`, and hello specified virtual directory is named `mylogblobs`, then hello log location would be `https://myaccount.blob.core.windows.net/mylogcontainer/waimportexport/mylogblobs`.</span></span>|  
  
<span data-ttu-id="fd296-133">Adresy URL hello hello chyba a podrobné protokoly můžete načíst pomocí volání hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operaci.</span><span class="sxs-lookup"><span data-stu-id="fd296-133">You can retrieve hello URL for hello error and verbose logs by calling hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operation.</span></span> <span data-ttu-id="fd296-134">Hello protokoly jsou k dispozici po dokončení zpracování jednotky hello.</span><span class="sxs-lookup"><span data-stu-id="fd296-134">hello logs are available after processing of hello drive is complete.</span></span>  
  
## <a name="log-file-format"></a><span data-ttu-id="fd296-135">Formát souboru protokolu</span><span class="sxs-lookup"><span data-stu-id="fd296-135">Log file format</span></span>  
<span data-ttu-id="fd296-136">Hello formát pro oba protokoly je hello stejné: Objekt blob, který obsahuje XML popis hello událostí, které došlo k chybě při kopírování objekty BLOB mezi hello pevný disk a účet hello zákazníka.</span><span class="sxs-lookup"><span data-stu-id="fd296-136">hello format for both logs is hello same: a blob containing XML descriptions of hello events that occurred while copying blobs between hello hard drive and hello customer's account.</span></span>  
  
<span data-ttu-id="fd296-137">Úplné protokolování Hello obsahuje kompletní informace o stavu hello hello kopírování pro každý objekt blob (pro úlohy importu) nebo soubor (pro úlohy exportu), zatímco hello chyba protokol obsahuje jenom hello informace pro objekty BLOB nebo soubory, které se vyskytly chyby během hello importovat nebo exportovat úlohy.</span><span class="sxs-lookup"><span data-stu-id="fd296-137">hello verbose log contains complete information about hello status of hello copy operation for every blob (for an import job) or file (for an export job), whereas hello error log contains only hello information for blobs or files that encountered errors during hello import or export job.</span></span>  
  
<span data-ttu-id="fd296-138">Formát Hello podrobného protokolování jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="fd296-138">hello verbose log format is shown below.</span></span> <span data-ttu-id="fd296-139">Protokol chyb Hello má hello stejné struktury, ale filtruje úspěšné operace.</span><span class="sxs-lookup"><span data-stu-id="fd296-139">hello error log has hello same structure, but filters out successful operations.</span></span>  

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

<span data-ttu-id="fd296-140">Hello následující tabulka popisuje elementy hello hello souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="fd296-140">hello following table describes hello elements of hello log file.</span></span>  
  
|<span data-ttu-id="fd296-141">XML Element</span><span class="sxs-lookup"><span data-stu-id="fd296-141">XML Element</span></span>|<span data-ttu-id="fd296-142">Typ</span><span class="sxs-lookup"><span data-stu-id="fd296-142">Type</span></span>|<span data-ttu-id="fd296-143">Popis</span><span class="sxs-lookup"><span data-stu-id="fd296-143">Description</span></span>|  
|-----------------|----------|-----------------|  
|`DriveLog`|<span data-ttu-id="fd296-144">XML Element</span><span class="sxs-lookup"><span data-stu-id="fd296-144">XML Element</span></span>|<span data-ttu-id="fd296-145">Představuje jednotku protokolu.</span><span class="sxs-lookup"><span data-stu-id="fd296-145">Represents a drive log.</span></span>|  
|`Version`|<span data-ttu-id="fd296-146">Atribut, řetězec</span><span class="sxs-lookup"><span data-stu-id="fd296-146">Attribute, String</span></span>|<span data-ttu-id="fd296-147">Hello verzi formátu protokolu hello.</span><span class="sxs-lookup"><span data-stu-id="fd296-147">hello version of hello log format.</span></span>|  
|`DriveId`|<span data-ttu-id="fd296-148">Řetězec</span><span class="sxs-lookup"><span data-stu-id="fd296-148">String</span></span>|<span data-ttu-id="fd296-149">Hello jednotky hardwaru sériové číslo.</span><span class="sxs-lookup"><span data-stu-id="fd296-149">hello drive's hardware serial number.</span></span>|  
|`Status`|<span data-ttu-id="fd296-150">Řetězec</span><span class="sxs-lookup"><span data-stu-id="fd296-150">String</span></span>|<span data-ttu-id="fd296-151">Stav zpracování jednotky hello.</span><span class="sxs-lookup"><span data-stu-id="fd296-151">Status of hello drive processing.</span></span> <span data-ttu-id="fd296-152">V tématu hello `Drive Status Codes` tabulky níže Další informace.</span><span class="sxs-lookup"><span data-stu-id="fd296-152">See hello `Drive Status Codes` table below for more information.</span></span>|  
|`Blob`|<span data-ttu-id="fd296-153">Vnořené – element XML</span><span class="sxs-lookup"><span data-stu-id="fd296-153">Nested XML element</span></span>|<span data-ttu-id="fd296-154">Představuje objekt blob.</span><span class="sxs-lookup"><span data-stu-id="fd296-154">Represents a blob.</span></span>|  
|`Blob/BlobPath`|<span data-ttu-id="fd296-155">Řetězec</span><span class="sxs-lookup"><span data-stu-id="fd296-155">String</span></span>|<span data-ttu-id="fd296-156">identifikátor URI objektu hello blob Hello.</span><span class="sxs-lookup"><span data-stu-id="fd296-156">hello URI of hello blob.</span></span>|  
|`Blob/FilePath`|<span data-ttu-id="fd296-157">Řetězec</span><span class="sxs-lookup"><span data-stu-id="fd296-157">String</span></span>|<span data-ttu-id="fd296-158">Hello relativní cestu toohello soubor na disku hello.</span><span class="sxs-lookup"><span data-stu-id="fd296-158">hello relative path toohello file on hello drive.</span></span>|  
|`Blob/Snapshot`|<span data-ttu-id="fd296-159">Data a času</span><span class="sxs-lookup"><span data-stu-id="fd296-159">DateTime</span></span>|<span data-ttu-id="fd296-160">Hello snímek aktuální verze objektu blob hello pro úlohu exportu.</span><span class="sxs-lookup"><span data-stu-id="fd296-160">hello snapshot version of hello blob, for an export job only.</span></span>|  
|`Blob/Length`|<span data-ttu-id="fd296-161">Integer</span><span class="sxs-lookup"><span data-stu-id="fd296-161">Integer</span></span>|<span data-ttu-id="fd296-162">Hello celková délka objektu blob hello v bajtech.</span><span class="sxs-lookup"><span data-stu-id="fd296-162">hello total length of hello blob in bytes.</span></span>|  
|`Blob/LastModified`|<span data-ttu-id="fd296-163">Data a času</span><span class="sxs-lookup"><span data-stu-id="fd296-163">DateTime</span></span>|<span data-ttu-id="fd296-164">Hello datum a čas poslední změny tohoto objektu blob hello pro úlohu exportu.</span><span class="sxs-lookup"><span data-stu-id="fd296-164">hello date/time that hello blob was last modified, for an export job only.</span></span>|  
|`Blob/ImportDisposition`|<span data-ttu-id="fd296-165">Řetězec</span><span class="sxs-lookup"><span data-stu-id="fd296-165">String</span></span>|<span data-ttu-id="fd296-166">Hello importovat dispozice hello objekt blob pro úlohu importu.</span><span class="sxs-lookup"><span data-stu-id="fd296-166">hello import disposition of hello blob, for an import job only.</span></span>|  
|`Blob/ImportDisposition/@Status`|<span data-ttu-id="fd296-167">Atribut, řetězec</span><span class="sxs-lookup"><span data-stu-id="fd296-167">Attribute, String</span></span>|<span data-ttu-id="fd296-168">Stav Hello hello importovat dispozice.</span><span class="sxs-lookup"><span data-stu-id="fd296-168">hello status of hello import disposition.</span></span>|  
|`PageRangeList`|<span data-ttu-id="fd296-169">Vnořené – element XML</span><span class="sxs-lookup"><span data-stu-id="fd296-169">Nested XML element</span></span>|<span data-ttu-id="fd296-170">Představuje seznam rozsahů stránek pro objekt blob stránky.</span><span class="sxs-lookup"><span data-stu-id="fd296-170">Represents a list of page ranges for a page blob.</span></span>|  
|`PageRange`|<span data-ttu-id="fd296-171">XML element</span><span class="sxs-lookup"><span data-stu-id="fd296-171">XML element</span></span>|<span data-ttu-id="fd296-172">Představuje rozsahu stránek.</span><span class="sxs-lookup"><span data-stu-id="fd296-172">Represents a page range.</span></span>|  
|`PageRange/@Offset`|<span data-ttu-id="fd296-173">Atribut, celé číslo</span><span class="sxs-lookup"><span data-stu-id="fd296-173">Attribute, Integer</span></span>|<span data-ttu-id="fd296-174">Počáteční posun rozsahu stránku hello v objektu blob hello.</span><span class="sxs-lookup"><span data-stu-id="fd296-174">Starting offset of hello page range in hello blob.</span></span>|  
|`PageRange/@Length`|<span data-ttu-id="fd296-175">Atribut, celé číslo</span><span class="sxs-lookup"><span data-stu-id="fd296-175">Attribute, Integer</span></span>|<span data-ttu-id="fd296-176">Délka v bajtech hello rozsahu stránek.</span><span class="sxs-lookup"><span data-stu-id="fd296-176">Length in bytes of hello page range.</span></span>|  
|`PageRange/@Hash`|<span data-ttu-id="fd296-177">Atribut, řetězec</span><span class="sxs-lookup"><span data-stu-id="fd296-177">Attribute, String</span></span>|<span data-ttu-id="fd296-178">Kódováním Base16 hodnota hash MD5 rozsahu stránku hello.</span><span class="sxs-lookup"><span data-stu-id="fd296-178">Base16-encoded MD5 hash of hello page range.</span></span>|  
|`PageRange/@Status`|<span data-ttu-id="fd296-179">Atribut, řetězec</span><span class="sxs-lookup"><span data-stu-id="fd296-179">Attribute, String</span></span>|<span data-ttu-id="fd296-180">Stav zpracování hello rozsahu stránek.</span><span class="sxs-lookup"><span data-stu-id="fd296-180">Status of processing hello page range.</span></span>|  
|`BlockList`|<span data-ttu-id="fd296-181">Vnořené – element XML</span><span class="sxs-lookup"><span data-stu-id="fd296-181">Nested XML element</span></span>|<span data-ttu-id="fd296-182">Představuje seznam bloků pro objekt blob bloku.</span><span class="sxs-lookup"><span data-stu-id="fd296-182">Represents a list of blocks for a block blob.</span></span>|  
|`Block`|<span data-ttu-id="fd296-183">XML element</span><span class="sxs-lookup"><span data-stu-id="fd296-183">XML element</span></span>|<span data-ttu-id="fd296-184">Představuje blok.</span><span class="sxs-lookup"><span data-stu-id="fd296-184">Represents a block.</span></span>|  
|`Block/@Offset`|<span data-ttu-id="fd296-185">Atribut, celé číslo</span><span class="sxs-lookup"><span data-stu-id="fd296-185">Attribute, Integer</span></span>|<span data-ttu-id="fd296-186">Počáteční odsazení hello blok v objektu blob hello.</span><span class="sxs-lookup"><span data-stu-id="fd296-186">Starting offset of hello block in hello blob.</span></span>|  
|`Block/@Length`|<span data-ttu-id="fd296-187">Atribut, celé číslo</span><span class="sxs-lookup"><span data-stu-id="fd296-187">Attribute, Integer</span></span>|<span data-ttu-id="fd296-188">Délka v bajtech hello bloku.</span><span class="sxs-lookup"><span data-stu-id="fd296-188">Length in bytes of hello block.</span></span>|  
|`Block/@Id`|<span data-ttu-id="fd296-189">Atribut, řetězec</span><span class="sxs-lookup"><span data-stu-id="fd296-189">Attribute, String</span></span>|<span data-ttu-id="fd296-190">ID bloku Hello.</span><span class="sxs-lookup"><span data-stu-id="fd296-190">hello block ID.</span></span>|  
|`Block/@Hash`|<span data-ttu-id="fd296-191">Atribut, řetězec</span><span class="sxs-lookup"><span data-stu-id="fd296-191">Attribute, String</span></span>|<span data-ttu-id="fd296-192">Kódováním Base16 hodnota hash MD5 hello bloku.</span><span class="sxs-lookup"><span data-stu-id="fd296-192">Base16-encoded MD5 hash of hello block.</span></span>|  
|`Block/@Status`|<span data-ttu-id="fd296-193">Atribut, řetězec</span><span class="sxs-lookup"><span data-stu-id="fd296-193">Attribute, String</span></span>|<span data-ttu-id="fd296-194">Stav zpracování hello bloku.</span><span class="sxs-lookup"><span data-stu-id="fd296-194">Status of processing hello block.</span></span>|  
|`Metadata`|<span data-ttu-id="fd296-195">Vnořené – element XML</span><span class="sxs-lookup"><span data-stu-id="fd296-195">Nested XML element</span></span>|<span data-ttu-id="fd296-196">Představuje metadata objektu blob hello.</span><span class="sxs-lookup"><span data-stu-id="fd296-196">Represents hello blob's metadata.</span></span>|  
|`Metadata/@Status`|<span data-ttu-id="fd296-197">Atribut, řetězec</span><span class="sxs-lookup"><span data-stu-id="fd296-197">Attribute, String</span></span>|<span data-ttu-id="fd296-198">Stav zpracování hello metadata objektu blob.</span><span class="sxs-lookup"><span data-stu-id="fd296-198">Status of processing of hello blob metadata.</span></span>|  
|`Metadata/GlobalPath`|<span data-ttu-id="fd296-199">Řetězec</span><span class="sxs-lookup"><span data-stu-id="fd296-199">String</span></span>|<span data-ttu-id="fd296-200">Relativní cesta toohello globální metadata souboru.</span><span class="sxs-lookup"><span data-stu-id="fd296-200">Relative path toohello global metadata file.</span></span>|  
|`Metadata/GlobalPath/@Hash`|<span data-ttu-id="fd296-201">Atribut, řetězec</span><span class="sxs-lookup"><span data-stu-id="fd296-201">Attribute, String</span></span>|<span data-ttu-id="fd296-202">Kódováním Base16 hodnota hash MD5 hello globální metadata souboru.</span><span class="sxs-lookup"><span data-stu-id="fd296-202">Base16-encoded MD5 hash of hello global metadata file.</span></span>|  
|`Metadata/Path`|<span data-ttu-id="fd296-203">Řetězec</span><span class="sxs-lookup"><span data-stu-id="fd296-203">String</span></span>|<span data-ttu-id="fd296-204">Soubor metadat toohello relativní cestu.</span><span class="sxs-lookup"><span data-stu-id="fd296-204">Relative path toohello metadata file.</span></span>|  
|`Metadata/Path/@Hash`|<span data-ttu-id="fd296-205">Atribut, řetězec</span><span class="sxs-lookup"><span data-stu-id="fd296-205">Attribute, String</span></span>|<span data-ttu-id="fd296-206">Kódováním Base16 hodnota hash MD5 hello metadata souboru.</span><span class="sxs-lookup"><span data-stu-id="fd296-206">Base16-encoded MD5 hash of hello metadata file.</span></span>|  
|`Properties`|<span data-ttu-id="fd296-207">Vnořené – element XML</span><span class="sxs-lookup"><span data-stu-id="fd296-207">Nested XML element</span></span>|<span data-ttu-id="fd296-208">Představuje vlastnosti objektu blob hello.</span><span class="sxs-lookup"><span data-stu-id="fd296-208">Represents hello blob properties.</span></span>|  
|`Properties/@Status`|<span data-ttu-id="fd296-209">Atribut, řetězec</span><span class="sxs-lookup"><span data-stu-id="fd296-209">Attribute, String</span></span>|<span data-ttu-id="fd296-210">Stav zpracování hello vlastnosti objektů blob, například soubor nebyl nalezen, byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="fd296-210">Status of processing hello blob properties, e.g. file not found, completed.</span></span>|  
|`Properties/GlobalPath`|<span data-ttu-id="fd296-211">Řetězec</span><span class="sxs-lookup"><span data-stu-id="fd296-211">String</span></span>|<span data-ttu-id="fd296-212">Relativní cesta toohello globální vlastnosti souboru.</span><span class="sxs-lookup"><span data-stu-id="fd296-212">Relative path toohello global properties file.</span></span>|  
|`Properties/GlobalPath/@Hash`|<span data-ttu-id="fd296-213">Atribut, řetězec</span><span class="sxs-lookup"><span data-stu-id="fd296-213">Attribute, String</span></span>|<span data-ttu-id="fd296-214">Kódováním Base16 hodnota hash MD5 hello globální vlastnosti souboru.</span><span class="sxs-lookup"><span data-stu-id="fd296-214">Base16-encoded MD5 hash of hello global properties file.</span></span>|  
|`Properties/Path`|<span data-ttu-id="fd296-215">Řetězec</span><span class="sxs-lookup"><span data-stu-id="fd296-215">String</span></span>|<span data-ttu-id="fd296-216">Relativní cesta toohello vlastnosti souboru.</span><span class="sxs-lookup"><span data-stu-id="fd296-216">Relative path toohello properties file.</span></span>|  
|`Properties/Path/@Hash`|<span data-ttu-id="fd296-217">Atribut, řetězec</span><span class="sxs-lookup"><span data-stu-id="fd296-217">Attribute, String</span></span>|<span data-ttu-id="fd296-218">Kódováním Base16 hodnota hash MD5 hello vlastnosti souboru.</span><span class="sxs-lookup"><span data-stu-id="fd296-218">Base16-encoded MD5 hash of hello properties file.</span></span>|  
|`Blob/Status`|<span data-ttu-id="fd296-219">Řetězec</span><span class="sxs-lookup"><span data-stu-id="fd296-219">String</span></span>|<span data-ttu-id="fd296-220">Stav zpracování objektu blob hello.</span><span class="sxs-lookup"><span data-stu-id="fd296-220">Status of processing hello blob.</span></span>|  
  
# <a name="drive-status-codes"></a><span data-ttu-id="fd296-221">Jednotka stavové kódy</span><span class="sxs-lookup"><span data-stu-id="fd296-221">Drive status codes</span></span>  
<span data-ttu-id="fd296-222">Hello následující tabulka uvádí hello stavové kódy pro zpracování na jednotku.</span><span class="sxs-lookup"><span data-stu-id="fd296-222">hello following table lists hello status codes for processing a drive.</span></span>  
  
|<span data-ttu-id="fd296-223">Stavový kód</span><span class="sxs-lookup"><span data-stu-id="fd296-223">Status code</span></span>|<span data-ttu-id="fd296-224">Popis</span><span class="sxs-lookup"><span data-stu-id="fd296-224">Description</span></span>|  
|-----------------|-----------------|  
|`Completed`|<span data-ttu-id="fd296-225">jednotka Hello dokončil zpracování bez chyb.</span><span class="sxs-lookup"><span data-stu-id="fd296-225">hello drive has finished processing without any errors.</span></span>|  
|`CompletedWithWarnings`|<span data-ttu-id="fd296-226">jednotka Hello dokončení zpracování se upozornění v jedné nebo více objektů blob na potížemi import hello zadaný pro objekty BLOB hello.</span><span class="sxs-lookup"><span data-stu-id="fd296-226">hello drive has finished processing with warnings in one or more blobs per hello import dispositions specified for hello blobs.</span></span>|  
|`CompletedWithErrors`|<span data-ttu-id="fd296-227">jednotka Hello bylo dokončeno s chybami v jedné nebo více objektů BLOB nebo bloky.</span><span class="sxs-lookup"><span data-stu-id="fd296-227">hello drive has finished with errors in one or more blobs or chunks.</span></span>|  
|`DiskNotFound`|<span data-ttu-id="fd296-228">Žádný disk nachází na jednotce hello.</span><span class="sxs-lookup"><span data-stu-id="fd296-228">No disk is found on hello drive.</span></span>|  
|`VolumeNotNtfs`|<span data-ttu-id="fd296-229">Hello první datový svazek na disku hello není ve formátu systému souborů NTFS.</span><span class="sxs-lookup"><span data-stu-id="fd296-229">hello first data volume on hello disk is not in NTFS format.</span></span>|  
|`DiskOperationFailed`|<span data-ttu-id="fd296-230">Při provádění operací na jednotce hello došlo k neznámé chybě.</span><span class="sxs-lookup"><span data-stu-id="fd296-230">An unknown failure occurred when performing operations on hello drive.</span></span>|  
|`BitLockerVolumeNotFound`|<span data-ttu-id="fd296-231">Nebyla nalezena žádná encryptable svazku Bitlockeru.</span><span class="sxs-lookup"><span data-stu-id="fd296-231">No BitLocker encryptable volume is found.</span></span>|  
|`BitLockerNotActivated`|<span data-ttu-id="fd296-232">BitLocker není na svazku hello povolené.</span><span class="sxs-lookup"><span data-stu-id="fd296-232">BitLocker is not enabled on hello volume.</span></span>|  
|`BitLockerProtectorNotFound`|<span data-ttu-id="fd296-233">ochranné zařízení klíče Hello číselné heslo na svazku hello neexistuje.</span><span class="sxs-lookup"><span data-stu-id="fd296-233">hello numerical password key protector does not exist on hello volume.</span></span>|  
|`BitLockerKeyInvalid`|<span data-ttu-id="fd296-234">Hello číselné heslo zadané nemůžete odemknout hello svazku.</span><span class="sxs-lookup"><span data-stu-id="fd296-234">hello numerical password provided cannot unlock hello volume.</span></span>|  
|`BitLockerUnlockVolumeFailed`|<span data-ttu-id="fd296-235">Došlo k neznámé chybě došlo při pokusu o toounlock hello svazku.</span><span class="sxs-lookup"><span data-stu-id="fd296-235">Unknown failure has happened when trying toounlock hello volume.</span></span>|  
|`BitLockerFailed`|<span data-ttu-id="fd296-236">Při provádění operací se BitLocker došlo k neznámé chybě.</span><span class="sxs-lookup"><span data-stu-id="fd296-236">An unknown failure occurred while performing BitLocker operations.</span></span>|  
|`ManifestNameInvalid`|<span data-ttu-id="fd296-237">Název souboru manifestu Hello je neplatný.</span><span class="sxs-lookup"><span data-stu-id="fd296-237">hello manifest file name is invalid.</span></span>|  
|`ManifestNameTooLong`|<span data-ttu-id="fd296-238">Název souboru manifestu Hello je příliš dlouhý.</span><span class="sxs-lookup"><span data-stu-id="fd296-238">hello manifest file name is too long.</span></span>|  
|`ManifestNotFound`|<span data-ttu-id="fd296-239">Hello soubor manifestu nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="fd296-239">hello manifest file is not found.</span></span>|  
|`ManifestAccessDenied`|<span data-ttu-id="fd296-240">Soubor manifestu toohello přístup byl odepřen.</span><span class="sxs-lookup"><span data-stu-id="fd296-240">Access toohello manifest file is denied.</span></span>|  
|`ManifestCorrupted`|<span data-ttu-id="fd296-241">Hello souboru manifestu je poškozený (hello obsah neodpovídá jeho hash).</span><span class="sxs-lookup"><span data-stu-id="fd296-241">hello manifest file is corrupted (hello content does not match its hash).</span></span>|  
|`ManifestFormatInvalid`|<span data-ttu-id="fd296-242">Obsah manifestu Hello nesplňuje požadovaný formát toohello.</span><span class="sxs-lookup"><span data-stu-id="fd296-242">hello manifest content does not conform toohello required format.</span></span>|  
|`ManifestDriveIdMismatch`|<span data-ttu-id="fd296-243">Hello jednotku, kterou neodpovídá ID v souboru manifestu hello hello jeden pro čtení z disku hello.</span><span class="sxs-lookup"><span data-stu-id="fd296-243">hello drive ID in hello manifest file does not match hello one read from hello drive.</span></span>|  
|`ReadManifestFailed`|<span data-ttu-id="fd296-244">Při čtení z manifestu hello došlo k vstupně-výstupních operací selhání disku.</span><span class="sxs-lookup"><span data-stu-id="fd296-244">A disk I/O failure occurred while reading from hello manifest.</span></span>|  
|`BlobListFormatInvalid`|<span data-ttu-id="fd296-245">Hello export objektů blob seznamu objektů blob nesplňuje požadovaný formát toohello.</span><span class="sxs-lookup"><span data-stu-id="fd296-245">hello export blob list blob does not conform toohello required format.</span></span>|  
|`BlobRequestForbidden`|<span data-ttu-id="fd296-246">Přístup k objektům BLOB toohello v účtu úložiště hello je zakázáno.</span><span class="sxs-lookup"><span data-stu-id="fd296-246">Access toohello blobs in hello storage account is forbidden.</span></span> <span data-ttu-id="fd296-247">Může to být kvůli tooinvalid klíč účtu úložiště nebo sdíleného přístupového podpisu kontejneru.</span><span class="sxs-lookup"><span data-stu-id="fd296-247">This might be due tooinvalid storage account key or container SAS.</span></span>|  
|`InternalError`|<span data-ttu-id="fd296-248">A při zpracování hello jednotce došlo k vnitřní chybě.</span><span class="sxs-lookup"><span data-stu-id="fd296-248">And internal error occurred while processing hello drive.</span></span>|  
  
## <a name="blob-status-codes"></a><span data-ttu-id="fd296-249">Objekt BLOB stavové kódy</span><span class="sxs-lookup"><span data-stu-id="fd296-249">Blob status codes</span></span>  
<span data-ttu-id="fd296-250">Hello následující tabulka uvádí hello stavové kódy pro zpracování objektu blob.</span><span class="sxs-lookup"><span data-stu-id="fd296-250">hello following table lists hello status codes for processing a blob.</span></span>  
  
|<span data-ttu-id="fd296-251">Stavový kód</span><span class="sxs-lookup"><span data-stu-id="fd296-251">Status code</span></span>|<span data-ttu-id="fd296-252">Popis</span><span class="sxs-lookup"><span data-stu-id="fd296-252">Description</span></span>|  
|-----------------|-----------------|  
|`Completed`|<span data-ttu-id="fd296-253">Objekt blob Hello dokončil zpracování bez chyb.</span><span class="sxs-lookup"><span data-stu-id="fd296-253">hello blob has finished processing without errors.</span></span>|  
|`CompletedWithErrors`|<span data-ttu-id="fd296-254">Objekt blob Hello dokončil zpracování s chybami v jedné nebo více rozsahů stránek nebo bloky, metadata nebo vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="fd296-254">hello blob has finished processing with errors in one or more page ranges or blocks, metadata, or properties.</span></span>|  
|`FileNameInvalid`|<span data-ttu-id="fd296-255">Název souboru Hello je neplatný.</span><span class="sxs-lookup"><span data-stu-id="fd296-255">hello file name is invalid.</span></span>|  
|`FileNameTooLong`|<span data-ttu-id="fd296-256">Název souboru Hello je příliš dlouhý.</span><span class="sxs-lookup"><span data-stu-id="fd296-256">hello file name is too long.</span></span>|  
|`FileNotFound`|<span data-ttu-id="fd296-257">Hello soubor nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="fd296-257">hello file is not found.</span></span>|  
|`FileAccessDenied`|<span data-ttu-id="fd296-258">Soubor toohello přístup byl odepřen.</span><span class="sxs-lookup"><span data-stu-id="fd296-258">Access toohello file is denied.</span></span>|  
|`BlobRequestFailed`|<span data-ttu-id="fd296-259">požadavek na službu Blob Hello, že tooaccess hello objekt blob se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="fd296-259">hello Blob service request tooaccess hello blob has failed.</span></span>|  
|`BlobRequestForbidden`|<span data-ttu-id="fd296-260">požadavek na službu Blob Hello, že tooaccess hello objektu blob je zakázán.</span><span class="sxs-lookup"><span data-stu-id="fd296-260">hello Blob service request tooaccess hello blob is forbidden.</span></span> <span data-ttu-id="fd296-261">Může to být kvůli tooinvalid klíč účtu úložiště nebo sdíleného přístupového podpisu kontejneru.</span><span class="sxs-lookup"><span data-stu-id="fd296-261">This might be due tooinvalid storage account key or container SAS.</span></span>|  
|`RenameFailed`|<span data-ttu-id="fd296-262">Objekt blob hello toorename (pro úlohy importu) nebo soubor hello (pro úlohy exportu) se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="fd296-262">Failed toorename hello blob (for an import job) or hello file (for an export job).</span></span>|  
|`BlobUnexpectedChange`|<span data-ttu-id="fd296-263">S objektem blob hello (pro úlohy exportu) došlo k neočekávané chování.</span><span class="sxs-lookup"><span data-stu-id="fd296-263">An unexpected change has occurred with hello blob (for an export job).</span></span>|  
|`LeasePresent`|<span data-ttu-id="fd296-264">Není přítomen u objektu blob hello zapůjčení.</span><span class="sxs-lookup"><span data-stu-id="fd296-264">There is a lease present on hello blob.</span></span>|  
|`IOFailed`|<span data-ttu-id="fd296-265">Na disk nebo sítě vstupně-výstupních operací selhání došlo k chybě při zpracování objektu blob hello.</span><span class="sxs-lookup"><span data-stu-id="fd296-265">A disk or network I/O failure occurred while processing hello blob.</span></span>|  
|`Failed`|<span data-ttu-id="fd296-266">Při zpracování objektu blob hello došlo k neznámé chybě.</span><span class="sxs-lookup"><span data-stu-id="fd296-266">An unknown failure occurred while processing hello blob.</span></span>|  
  
## <a name="import-disposition-status-codes"></a><span data-ttu-id="fd296-267">Import dispozice stavové kódy</span><span class="sxs-lookup"><span data-stu-id="fd296-267">Import disposition status codes</span></span>  
<span data-ttu-id="fd296-268">Hello následující tabulka uvádí hello stavové kódy pro řešení importu dispozice.</span><span class="sxs-lookup"><span data-stu-id="fd296-268">hello following table lists hello status codes for resolving an import disposition.</span></span>  
  
|<span data-ttu-id="fd296-269">Stavový kód</span><span class="sxs-lookup"><span data-stu-id="fd296-269">Status code</span></span>|<span data-ttu-id="fd296-270">Popis</span><span class="sxs-lookup"><span data-stu-id="fd296-270">Description</span></span>|  
|-----------------|-----------------|  
|`Created`|<span data-ttu-id="fd296-271">vytvořil se objekt blob Hello.</span><span class="sxs-lookup"><span data-stu-id="fd296-271">hello blob has been created.</span></span>|  
|`Renamed`|<span data-ttu-id="fd296-272">Hello blob byl přejmenován na přejmenování import dispozice.</span><span class="sxs-lookup"><span data-stu-id="fd296-272">hello blob has been renamed per rename import disposition.</span></span> <span data-ttu-id="fd296-273">Hello `Blob/BlobPath` element obsahuje hello identifikátor URI pro objekt blob hello přejmenovat.</span><span class="sxs-lookup"><span data-stu-id="fd296-273">hello `Blob/BlobPath` element contains hello URI for hello renamed blob.</span></span>|  
|`Skipped`|<span data-ttu-id="fd296-274">Objekt blob Hello byl vynechán za `no-overwrite` importovat dispozice.</span><span class="sxs-lookup"><span data-stu-id="fd296-274">hello blob has been skipped per `no-overwrite` import disposition.</span></span>|  
|`Overwritten`|<span data-ttu-id="fd296-275">Objekt blob Hello má přepsat existující objekt blob za `overwrite` importovat dispozice.</span><span class="sxs-lookup"><span data-stu-id="fd296-275">hello blob has overwritten an existing blob per `overwrite` import disposition.</span></span>|  
|`Cancelled`|<span data-ttu-id="fd296-276">Předchozí selhání byla zastavena, další zpracování hello import dispozice.</span><span class="sxs-lookup"><span data-stu-id="fd296-276">A prior failure has stopped further processing of hello import disposition.</span></span>|  
  
## <a name="page-rangeblock-status-codes"></a><span data-ttu-id="fd296-277">Stránka rozsah/blok stavové kódy</span><span class="sxs-lookup"><span data-stu-id="fd296-277">Page range/block status codes</span></span>  
<span data-ttu-id="fd296-278">Hello následující tabulka uvádí hello stavové kódy pro zpracování stránky rozsah nebo blok.</span><span class="sxs-lookup"><span data-stu-id="fd296-278">hello following table lists hello status codes for processing a page range or a block.</span></span>  
  
|<span data-ttu-id="fd296-279">Stavový kód</span><span class="sxs-lookup"><span data-stu-id="fd296-279">Status code</span></span>|<span data-ttu-id="fd296-280">Popis</span><span class="sxs-lookup"><span data-stu-id="fd296-280">Description</span></span>|  
|-----------------|-----------------|  
|`Completed`|<span data-ttu-id="fd296-281">Hello stránky rozsah nebo blok dokončení zpracování bez chyb.</span><span class="sxs-lookup"><span data-stu-id="fd296-281">hello page range or block has finished processing without any errors.</span></span>|  
|`Committed`|<span data-ttu-id="fd296-282">blok Hello potvrzeny, ale v hello úplné blokovaných protože jiné bloky se nezdařily, nebo put samotný seznam úplné bloku nedošlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="fd296-282">hello block has been committed,  but not in hello full block list because other blocks have failed, or put full block list itself has failed.</span></span>|  
|`Uncommitted`|<span data-ttu-id="fd296-283">Hello bloku je odeslat, ale nikoli potvrdit.</span><span class="sxs-lookup"><span data-stu-id="fd296-283">hello block is uploaded but not committed.</span></span>|  
|`Corrupted`|<span data-ttu-id="fd296-284">Hello stránky rozsah nebo blok je poškozený (hello obsah neodpovídá jeho hash).</span><span class="sxs-lookup"><span data-stu-id="fd296-284">hello page range or block is corrupted (hello content does not match its hash).</span></span>|  
|`FileUnexpectedEnd`|<span data-ttu-id="fd296-285">Byl nalezen neočekávaný konec souboru.</span><span class="sxs-lookup"><span data-stu-id="fd296-285">An unexpected end of file has been encountered.</span></span>|  
|`BlobUnexpectedEnd`|<span data-ttu-id="fd296-286">Byl nalezen neočekávaný konec objektu blob.</span><span class="sxs-lookup"><span data-stu-id="fd296-286">An unexpected end of blob has been encountered.</span></span>|  
|`BlobRequestFailed`|<span data-ttu-id="fd296-287">Hello žádost o služby objektů Blob, že tooaccess hello rozsahu stránek nebo bloku selhal.</span><span class="sxs-lookup"><span data-stu-id="fd296-287">hello Blob service request tooaccess hello page range or block has failed.</span></span>|  
|`IOFailed`|<span data-ttu-id="fd296-288">Na disk nebo síťový vstupně-výstupní chybě došlo k chybě při zpracování hello stránky rozsah nebo blok.</span><span class="sxs-lookup"><span data-stu-id="fd296-288">A disk or network I/O failure occurred while processing hello page range or block.</span></span>|  
|`Failed`|<span data-ttu-id="fd296-289">Při zpracování hello stránky rozsah nebo blok došlo k neznámé chybě.</span><span class="sxs-lookup"><span data-stu-id="fd296-289">An unknown failure occurred while processing hello page range or block.</span></span>|  
|`Cancelled`|<span data-ttu-id="fd296-290">Předchozí selhání byla zastavena, další zpracování rozsahu stránek hello nebo bloku.</span><span class="sxs-lookup"><span data-stu-id="fd296-290">A prior failure has stopped further processing of hello page range or block.</span></span>|  
  
## <a name="metadata-status-codes"></a><span data-ttu-id="fd296-291">Metadata stavové kódy</span><span class="sxs-lookup"><span data-stu-id="fd296-291">Metadata status codes</span></span>  
<span data-ttu-id="fd296-292">Hello následující tabulka uvádí hello stavové kódy pro zpracování metadata objektu blob.</span><span class="sxs-lookup"><span data-stu-id="fd296-292">hello following table lists hello status codes for processing blob metadata.</span></span>  
  
|<span data-ttu-id="fd296-293">Stavový kód</span><span class="sxs-lookup"><span data-stu-id="fd296-293">Status code</span></span>|<span data-ttu-id="fd296-294">Popis</span><span class="sxs-lookup"><span data-stu-id="fd296-294">Description</span></span>|  
|-----------------|-----------------|  
|`Completed`|<span data-ttu-id="fd296-295">Hello metadata dokončil zpracování bez chyb.</span><span class="sxs-lookup"><span data-stu-id="fd296-295">hello metadata has finished processing without errors.</span></span>|  
|`FileNameInvalid`|<span data-ttu-id="fd296-296">Název souboru metadat Hello je neplatný.</span><span class="sxs-lookup"><span data-stu-id="fd296-296">hello metadata file name is invalid.</span></span>|  
|`FileNameTooLong`|<span data-ttu-id="fd296-297">Název souboru metadat Hello je příliš dlouhý.</span><span class="sxs-lookup"><span data-stu-id="fd296-297">hello metadata file name is too long.</span></span>|  
|`FileNotFound`|<span data-ttu-id="fd296-298">Soubor metadat Hello nebyla nalezena.</span><span class="sxs-lookup"><span data-stu-id="fd296-298">hello metadata file is not found.</span></span>|  
|`FileAccessDenied`|<span data-ttu-id="fd296-299">Soubor metadat toohello přístup byl odepřen.</span><span class="sxs-lookup"><span data-stu-id="fd296-299">Access toohello metadata file is denied.</span></span>|  
|`Corrupted`|<span data-ttu-id="fd296-300">je poškozený soubor metadat Hello (hello obsah neodpovídá jeho hash).</span><span class="sxs-lookup"><span data-stu-id="fd296-300">hello metadata file is corrupted (hello content does not match its hash).</span></span>|  
|`XmlReadFailed`|<span data-ttu-id="fd296-301">Hello metadata obsahu neodpovídá toohello požadovaný formát.</span><span class="sxs-lookup"><span data-stu-id="fd296-301">hello metadata content does not conform toohello required format.</span></span>|  
|`XmlWriteFailed`|<span data-ttu-id="fd296-302">Zápis metadat hello XML se nezdařila.</span><span class="sxs-lookup"><span data-stu-id="fd296-302">Writing hello metadata XML has failed.</span></span>|  
|`BlobRequestFailed`|<span data-ttu-id="fd296-303">Hello Blob žádosti o službu, že tooaccess hello metadat se nezdařila.</span><span class="sxs-lookup"><span data-stu-id="fd296-303">hello Blob service request tooaccess hello metadata has failed.</span></span>|  
|`IOFailed`|<span data-ttu-id="fd296-304">Při zpracování hello metadat došlo k chybě vstupně-výstupních operací disku nebo v síti.</span><span class="sxs-lookup"><span data-stu-id="fd296-304">A disk or network I/O failure occurred while processing hello metadata.</span></span>|  
|`Failed`|<span data-ttu-id="fd296-305">Při zpracování metadat hello došlo k neznámé chybě.</span><span class="sxs-lookup"><span data-stu-id="fd296-305">An unknown failure occurred while processing hello metadata.</span></span>|  
|`Cancelled`|<span data-ttu-id="fd296-306">Předchozí selhání byla zastavena, další zpracování hello metadat.</span><span class="sxs-lookup"><span data-stu-id="fd296-306">A prior failure has stopped further processing of hello metadata.</span></span>|  
  
## <a name="properties-status-codes"></a><span data-ttu-id="fd296-307">Vlastnosti stavové kódy</span><span class="sxs-lookup"><span data-stu-id="fd296-307">Properties status codes</span></span>  
<span data-ttu-id="fd296-308">Hello následující tabulka uvádí hello stavové kódy pro zpracování vlastnosti objektů blob.</span><span class="sxs-lookup"><span data-stu-id="fd296-308">hello following table lists hello status codes for processing blob properties.</span></span>  
  
|<span data-ttu-id="fd296-309">Stavový kód</span><span class="sxs-lookup"><span data-stu-id="fd296-309">Status code</span></span>|<span data-ttu-id="fd296-310">Popis</span><span class="sxs-lookup"><span data-stu-id="fd296-310">Description</span></span>|  
|-----------------|-----------------|  
|`Completed`|<span data-ttu-id="fd296-311">Vlastnosti Hello dokončení zpracování bez chyb.</span><span class="sxs-lookup"><span data-stu-id="fd296-311">hello properties have finished processing without any errors.</span></span>|  
|`FileNameInvalid`|<span data-ttu-id="fd296-312">Název souboru Hello vlastnosti je neplatný.</span><span class="sxs-lookup"><span data-stu-id="fd296-312">hello properties file name is invalid.</span></span>|  
|`FileNameTooLong`|<span data-ttu-id="fd296-313">Název souboru vlastnosti Hello je příliš dlouhý.</span><span class="sxs-lookup"><span data-stu-id="fd296-313">hello properties file name is too long.</span></span>|  
|`FileNotFound`|<span data-ttu-id="fd296-314">Hello vlastnosti soubor nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="fd296-314">hello properties file is not found.</span></span>|  
|`FileAccessDenied`|<span data-ttu-id="fd296-315">Soubor vlastnosti toohello přístup byl odepřen.</span><span class="sxs-lookup"><span data-stu-id="fd296-315">Access toohello properties file is denied.</span></span>|  
|`Corrupted`|<span data-ttu-id="fd296-316">Hello vlastnosti soubor je poškozený (hello obsah neodpovídá jeho hash).</span><span class="sxs-lookup"><span data-stu-id="fd296-316">hello properties file is corrupted (hello content does not match its hash).</span></span>|  
|`XmlReadFailed`|<span data-ttu-id="fd296-317">Hello vlastnosti obsah není v souladu s toohello požadovaný formát.</span><span class="sxs-lookup"><span data-stu-id="fd296-317">hello properties content does not conform toohello required format.</span></span>|  
|`XmlWriteFailed`|<span data-ttu-id="fd296-318">Zápis hello vlastnosti, které XML se nezdařila.</span><span class="sxs-lookup"><span data-stu-id="fd296-318">Writing hello properties XML has failed.</span></span>|  
|`BlobRequestFailed`|<span data-ttu-id="fd296-319">požadavek na službu Blob Hello, že tooaccess hello vlastnosti se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="fd296-319">hello Blob service request tooaccess hello properties has failed.</span></span>|  
|`IOFailed`|<span data-ttu-id="fd296-320">Při zpracování hello vlastnosti došlo k chybě vstupně-výstupních operací disku nebo v síti.</span><span class="sxs-lookup"><span data-stu-id="fd296-320">A disk or network I/O failure occurred while processing hello properties.</span></span>|  
|`Failed`|<span data-ttu-id="fd296-321">Při zpracování hello vlastnosti došlo k neznámé chybě.</span><span class="sxs-lookup"><span data-stu-id="fd296-321">An unknown failure occurred while processing hello properties.</span></span>|  
|`Cancelled`|<span data-ttu-id="fd296-322">Předchozí selhání byla zastavena, další zpracování hello vlastností.</span><span class="sxs-lookup"><span data-stu-id="fd296-322">A prior failure has stopped further processing of hello properties.</span></span>|  
  
## <a name="sample-logs"></a><span data-ttu-id="fd296-323">Ukázka protokoly</span><span class="sxs-lookup"><span data-stu-id="fd296-323">Sample logs</span></span>  
<span data-ttu-id="fd296-324">Hello následuje příklad podrobného protokolování.</span><span class="sxs-lookup"><span data-stu-id="fd296-324">hello following is an example of verbose log.</span></span>  
  
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
  
<span data-ttu-id="fd296-325">Protokol chyb odpovídající Hello je uveden níže.</span><span class="sxs-lookup"><span data-stu-id="fd296-325">hello corresponding error log is shown below.</span></span>  
  
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

 <span data-ttu-id="fd296-326">Hello postupujte podle kroků v protokolu chyb úlohy importu obsahuje chybu o soubor nebyl nalezen na disku import hello.</span><span class="sxs-lookup"><span data-stu-id="fd296-326">hello follow error log for an import job contains an error about a file not found on hello import drive.</span></span> <span data-ttu-id="fd296-327">Upozorňujeme, že se stav hello následující součásti `Cancelled`.</span><span class="sxs-lookup"><span data-stu-id="fd296-327">Note that hello status of subsequent components is `Cancelled`.</span></span>  
  
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

<span data-ttu-id="fd296-328">Hello následující v protokolu chyb úlohy exportu označuje, že hello obsah objektu blob byla úspěšně zapsána toohello disku, ale, že došlo k chybě při exportu hello blob vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="fd296-328">hello following error log for an export job indicates that hello blob content has been successfully written toohello drive, but that an error occurred while exporting hello blob's properties.</span></span>  
  
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
  
## <a name="next-steps"></a><span data-ttu-id="fd296-329">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fd296-329">Next steps</span></span>
 
* [<span data-ttu-id="fd296-330">REST API pro Import/Export úložiště</span><span class="sxs-lookup"><span data-stu-id="fd296-330">Storage Import/Export REST API</span></span>](/rest/api/storageimportexport/)
