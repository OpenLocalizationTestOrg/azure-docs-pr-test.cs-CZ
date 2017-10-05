---
title: "Azure Import/Export metadat a vlastnosti formát souboru | Microsoft Docs"
description: "Zjistěte, jak určit metadata a vlastnosti pro jeden nebo více objektů BLOB, které jsou součástí importu nebo exportu úlohy."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 840364c6-d9a8-4b43-a9f3-f7441c625069
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 3f728ad94cdcbd32092b677f11a737ae91376720
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-importexport-service-metadata-and-properties-file-format"></a><span data-ttu-id="4b578-103">Azure Import/Export metadat a vlastnosti souboru formát služby</span><span class="sxs-lookup"><span data-stu-id="4b578-103">Azure Import/Export service metadata and properties file format</span></span>
<span data-ttu-id="4b578-104">Metadata a vlastnosti pro jeden nebo více objektů BLOB můžete zadat jako součást úlohy importu nebo úlohy exportu.</span><span class="sxs-lookup"><span data-stu-id="4b578-104">You can specify metadata and properties for one or more blobs as part of an import job or an export job.</span></span> <span data-ttu-id="4b578-105">Pokud chcete nastavit vlastnosti pro objekty BLOB vytváří jako součást úlohy importu nebo metadata, poskytnete metadata nebo vlastnosti souboru na pevný disk obsahující data, která bude importována.</span><span class="sxs-lookup"><span data-stu-id="4b578-105">To set metadata or properties for blobs being created as part of an import job, you provide a metadata or properties file on the hard drive containing the data to be imported.</span></span> <span data-ttu-id="4b578-106">Pro úlohy exportu metadat a vlastnosti se zapisují do metadata nebo vlastnosti souboru, který je zahrnut na pevný disk, který vrátil pro vás.</span><span class="sxs-lookup"><span data-stu-id="4b578-106">For an export job, metadata and properties are written to a metadata or properties file that is included on the hard drive returned to you.</span></span>  
  
## <a name="metadata-file-format"></a><span data-ttu-id="4b578-107">Formát souboru metadat</span><span class="sxs-lookup"><span data-stu-id="4b578-107">Metadata file format</span></span>  
<span data-ttu-id="4b578-108">Formát souboru metadat je následující:</span><span class="sxs-lookup"><span data-stu-id="4b578-108">The format of a metadata file is as follows:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
[<metadata-name-1>metadata-value-1</metadata-name-1>]  
[<metadata-name-2>metadata-value-2</metadata-name-2>]  
. . .  
</Metadata>  
```
  
|<span data-ttu-id="4b578-109">XML Element</span><span class="sxs-lookup"><span data-stu-id="4b578-109">XML Element</span></span>|<span data-ttu-id="4b578-110">Typ</span><span class="sxs-lookup"><span data-stu-id="4b578-110">Type</span></span>|<span data-ttu-id="4b578-111">Popis</span><span class="sxs-lookup"><span data-stu-id="4b578-111">Description</span></span>|  
|-----------------|----------|-----------------|  
|`Metadata`|<span data-ttu-id="4b578-112">Kořenový element</span><span class="sxs-lookup"><span data-stu-id="4b578-112">Root element</span></span>|<span data-ttu-id="4b578-113">Kořenový element soubor metadat.</span><span class="sxs-lookup"><span data-stu-id="4b578-113">The root element of the metadata file.</span></span>|  
|`metadata-name`|<span data-ttu-id="4b578-114">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4b578-114">String</span></span>|<span data-ttu-id="4b578-115">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="4b578-115">Optional.</span></span> <span data-ttu-id="4b578-116">XML element určuje název metadata pro objekt blob a jeho hodnota určuje hodnota nastavení metadat.</span><span class="sxs-lookup"><span data-stu-id="4b578-116">The XML element specifies the name of the metadata for the blob, and its value specifies the value of the metadata setting.</span></span>|  
  
## <a name="properties-file-format"></a><span data-ttu-id="4b578-117">Formát souboru vlastnosti</span><span class="sxs-lookup"><span data-stu-id="4b578-117">Properties file format</span></span>  
<span data-ttu-id="4b578-118">Formát souboru vlastnosti vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="4b578-118">The format of a properties file is as follows:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
[<Last-Modified>date-time-value</Last-Modified>]  
[<Etag>etag</Etag>]  
[<Content-Length>size-in-bytes<Content-Length>]  
[<Content-Type>content-type</Content-Type>]  
[<Content-MD5>content-md5</Content-MD5>]  
[<Content-Encoding>content-encoding</Content-Encoding>]  
[<Content-Language>content-language</Content-Language>]  
[<Cache-Control>cache-control</Cache-Control>]  
</Properties>  
```
  
|<span data-ttu-id="4b578-119">XML Element</span><span class="sxs-lookup"><span data-stu-id="4b578-119">XML Element</span></span>|<span data-ttu-id="4b578-120">Typ</span><span class="sxs-lookup"><span data-stu-id="4b578-120">Type</span></span>|<span data-ttu-id="4b578-121">Popis</span><span class="sxs-lookup"><span data-stu-id="4b578-121">Description</span></span>|  
|-----------------|----------|-----------------|  
|`Properties`|<span data-ttu-id="4b578-122">Kořenový element</span><span class="sxs-lookup"><span data-stu-id="4b578-122">Root element</span></span>|<span data-ttu-id="4b578-123">Kořenový element souboru vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="4b578-123">The root element of the properties file.</span></span>|  
|`Last-Modified`|<span data-ttu-id="4b578-124">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4b578-124">String</span></span>|<span data-ttu-id="4b578-125">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="4b578-125">Optional.</span></span> <span data-ttu-id="4b578-126">Čas poslední úpravy pro tento objekt blob.</span><span class="sxs-lookup"><span data-stu-id="4b578-126">The last-modified time for the blob.</span></span> <span data-ttu-id="4b578-127">Pro export pouze úlohy.</span><span class="sxs-lookup"><span data-stu-id="4b578-127">For export jobs only.</span></span>|  
|`Etag`|<span data-ttu-id="4b578-128">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4b578-128">String</span></span>|<span data-ttu-id="4b578-129">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="4b578-129">Optional.</span></span> <span data-ttu-id="4b578-130">Hodnota objektu blob značka ETag.</span><span class="sxs-lookup"><span data-stu-id="4b578-130">The blob's ETag value.</span></span> <span data-ttu-id="4b578-131">Pro export pouze úlohy.</span><span class="sxs-lookup"><span data-stu-id="4b578-131">For export jobs only.</span></span>|  
|`Content-Length`|<span data-ttu-id="4b578-132">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4b578-132">String</span></span>|<span data-ttu-id="4b578-133">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="4b578-133">Optional.</span></span> <span data-ttu-id="4b578-134">Velikost objektu blob v bajtech.</span><span class="sxs-lookup"><span data-stu-id="4b578-134">The size of the blob in bytes.</span></span> <span data-ttu-id="4b578-135">Pro export pouze úlohy.</span><span class="sxs-lookup"><span data-stu-id="4b578-135">For export jobs only.</span></span>|  
|`Content-Type`|<span data-ttu-id="4b578-136">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4b578-136">String</span></span>|<span data-ttu-id="4b578-137">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="4b578-137">Optional.</span></span> <span data-ttu-id="4b578-138">Typ obsahu objektu blob.</span><span class="sxs-lookup"><span data-stu-id="4b578-138">The content type of the blob.</span></span>|  
|`Content-MD5`|<span data-ttu-id="4b578-139">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4b578-139">String</span></span>|<span data-ttu-id="4b578-140">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="4b578-140">Optional.</span></span> <span data-ttu-id="4b578-141">Hodnota hash MD5 objektu blob.</span><span class="sxs-lookup"><span data-stu-id="4b578-141">The blob's MD5 hash.</span></span>|  
|`Content-Encoding`|<span data-ttu-id="4b578-142">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4b578-142">String</span></span>|<span data-ttu-id="4b578-143">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="4b578-143">Optional.</span></span> <span data-ttu-id="4b578-144">Obsah objektu blob kódování.</span><span class="sxs-lookup"><span data-stu-id="4b578-144">The blob's content encoding.</span></span>|  
|`Content-Language`|<span data-ttu-id="4b578-145">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4b578-145">String</span></span>|<span data-ttu-id="4b578-146">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="4b578-146">Optional.</span></span> <span data-ttu-id="4b578-147">Jazyk obsahu objektu blob.</span><span class="sxs-lookup"><span data-stu-id="4b578-147">The blob's content language.</span></span>|  
|`Cache-Control`|<span data-ttu-id="4b578-148">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4b578-148">String</span></span>|<span data-ttu-id="4b578-149">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="4b578-149">Optional.</span></span> <span data-ttu-id="4b578-150">Řetězec ovládací prvek mezipaměti pro tento objekt blob.</span><span class="sxs-lookup"><span data-stu-id="4b578-150">The cache control string for the blob.</span></span>|  

## <a name="next-steps"></a><span data-ttu-id="4b578-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4b578-151">Next steps</span></span>

<span data-ttu-id="4b578-152">V tématu [umožňuje nastavit vlastnosti objektu blob](/rest/api/storageservices/set-blob-properties), [nastavit Metadata objektu Blob](/rest/api/storageservices/set-blob-metadata), a [vlastnosti a metadata pro nastavení a načítání objektů blob prostředky](/rest/api/storageservices/setting-and-retrieving-properties-and-metadata-for-blob-resources) pro podrobná pravidla o nastavení vlastnosti a metadata objektu blob.</span><span class="sxs-lookup"><span data-stu-id="4b578-152">See [Set blob properties](/rest/api/storageservices/set-blob-properties), [Set Blob Metadata](/rest/api/storageservices/set-blob-metadata), and [Setting and retrieving properties and metadata for blob resources](/rest/api/storageservices/setting-and-retrieving-properties-and-metadata-for-blob-resources) for detailed rules about setting blob metadata and properties.</span></span>