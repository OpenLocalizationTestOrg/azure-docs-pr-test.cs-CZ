---
title: "aaaAzure importu a exportu metadat a vlastnosti souboru formátu | Microsoft Docs"
description: "Zjistěte, jak toospecify metadata a vlastnosti pro jeden nebo více objektů BLOB, jsou součástí importu nebo exportu úlohy."
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
ms.openlocfilehash: bb13c1f1a27baea77298cb224970cd521d02d8c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-importexport-service-metadata-and-properties-file-format"></a><span data-ttu-id="1c4e6-103">Azure Import/Export metadat a vlastnosti souboru formát služby</span><span class="sxs-lookup"><span data-stu-id="1c4e6-103">Azure Import/Export service metadata and properties file format</span></span>
<span data-ttu-id="1c4e6-104">Metadata a vlastnosti pro jeden nebo více objektů BLOB můžete zadat jako součást úlohy importu nebo úlohy exportu.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-104">You can specify metadata and properties for one or more blobs as part of an import job or an export job.</span></span> <span data-ttu-id="1c4e6-105">vlastnosti pro objekty BLOB vytváří jako součást úlohy importu nebo tooset metadata, je třeba zadat soubor metadat nebo vlastnosti na hello pevný disk obsahující toobe hello data importovat.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-105">tooset metadata or properties for blobs being created as part of an import job, you provide a metadata or properties file on hello hard drive containing hello data toobe imported.</span></span> <span data-ttu-id="1c4e6-106">Pro úlohy exportu metadat a vlastnosti jsou zapsány do tooa metadata nebo vlastnosti souboru, který je zahrnut na pevném disku hello vrátil tooyou.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-106">For an export job, metadata and properties are written tooa metadata or properties file that is included on hello hard drive returned tooyou.</span></span>  
  
## <a name="metadata-file-format"></a><span data-ttu-id="1c4e6-107">Formát souboru metadat</span><span class="sxs-lookup"><span data-stu-id="1c4e6-107">Metadata file format</span></span>  
<span data-ttu-id="1c4e6-108">Hello formát souboru metadat je následující:</span><span class="sxs-lookup"><span data-stu-id="1c4e6-108">hello format of a metadata file is as follows:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
[<metadata-name-1>metadata-value-1</metadata-name-1>]  
[<metadata-name-2>metadata-value-2</metadata-name-2>]  
. . .  
</Metadata>  
```
  
|<span data-ttu-id="1c4e6-109">XML Element</span><span class="sxs-lookup"><span data-stu-id="1c4e6-109">XML Element</span></span>|<span data-ttu-id="1c4e6-110">Typ</span><span class="sxs-lookup"><span data-stu-id="1c4e6-110">Type</span></span>|<span data-ttu-id="1c4e6-111">Popis</span><span class="sxs-lookup"><span data-stu-id="1c4e6-111">Description</span></span>|  
|-----------------|----------|-----------------|  
|`Metadata`|<span data-ttu-id="1c4e6-112">Kořenový element</span><span class="sxs-lookup"><span data-stu-id="1c4e6-112">Root element</span></span>|<span data-ttu-id="1c4e6-113">kořenový element Hello hello metadata souboru.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-113">hello root element of hello metadata file.</span></span>|  
|`metadata-name`|<span data-ttu-id="1c4e6-114">Řetězec</span><span class="sxs-lookup"><span data-stu-id="1c4e6-114">String</span></span>|<span data-ttu-id="1c4e6-115">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-115">Optional.</span></span> <span data-ttu-id="1c4e6-116">Hello – element XML určuje název hello hello metadata pro objekt blob hello a jeho hodnota určuje hello hodnotu nastavení metadata hello.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-116">hello XML element specifies hello name of hello metadata for hello blob, and its value specifies hello value of hello metadata setting.</span></span>|  
  
## <a name="properties-file-format"></a><span data-ttu-id="1c4e6-117">Formát souboru vlastnosti</span><span class="sxs-lookup"><span data-stu-id="1c4e6-117">Properties file format</span></span>  
<span data-ttu-id="1c4e6-118">Hello formát souboru vlastnosti vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="1c4e6-118">hello format of a properties file is as follows:</span></span>  
  
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
  
|<span data-ttu-id="1c4e6-119">XML Element</span><span class="sxs-lookup"><span data-stu-id="1c4e6-119">XML Element</span></span>|<span data-ttu-id="1c4e6-120">Typ</span><span class="sxs-lookup"><span data-stu-id="1c4e6-120">Type</span></span>|<span data-ttu-id="1c4e6-121">Popis</span><span class="sxs-lookup"><span data-stu-id="1c4e6-121">Description</span></span>|  
|-----------------|----------|-----------------|  
|`Properties`|<span data-ttu-id="1c4e6-122">Kořenový element</span><span class="sxs-lookup"><span data-stu-id="1c4e6-122">Root element</span></span>|<span data-ttu-id="1c4e6-123">kořenový element Hello hello vlastnosti souboru.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-123">hello root element of hello properties file.</span></span>|  
|`Last-Modified`|<span data-ttu-id="1c4e6-124">Řetězec</span><span class="sxs-lookup"><span data-stu-id="1c4e6-124">String</span></span>|<span data-ttu-id="1c4e6-125">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-125">Optional.</span></span> <span data-ttu-id="1c4e6-126">Hello čas poslední změny pro objekt blob hello.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-126">hello last-modified time for hello blob.</span></span> <span data-ttu-id="1c4e6-127">Pro export pouze úlohy.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-127">For export jobs only.</span></span>|  
|`Etag`|<span data-ttu-id="1c4e6-128">Řetězec</span><span class="sxs-lookup"><span data-stu-id="1c4e6-128">String</span></span>|<span data-ttu-id="1c4e6-129">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-129">Optional.</span></span> <span data-ttu-id="1c4e6-130">Hello hodnota ETag objektu blob.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-130">hello blob's ETag value.</span></span> <span data-ttu-id="1c4e6-131">Pro export pouze úlohy.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-131">For export jobs only.</span></span>|  
|`Content-Length`|<span data-ttu-id="1c4e6-132">Řetězec</span><span class="sxs-lookup"><span data-stu-id="1c4e6-132">String</span></span>|<span data-ttu-id="1c4e6-133">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-133">Optional.</span></span> <span data-ttu-id="1c4e6-134">Hello velikost objektu blob hello v bajtech.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-134">hello size of hello blob in bytes.</span></span> <span data-ttu-id="1c4e6-135">Pro export pouze úlohy.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-135">For export jobs only.</span></span>|  
|`Content-Type`|<span data-ttu-id="1c4e6-136">Řetězec</span><span class="sxs-lookup"><span data-stu-id="1c4e6-136">String</span></span>|<span data-ttu-id="1c4e6-137">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-137">Optional.</span></span> <span data-ttu-id="1c4e6-138">Typ obsahu Hello objektu hello blob.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-138">hello content type of hello blob.</span></span>|  
|`Content-MD5`|<span data-ttu-id="1c4e6-139">Řetězec</span><span class="sxs-lookup"><span data-stu-id="1c4e6-139">String</span></span>|<span data-ttu-id="1c4e6-140">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-140">Optional.</span></span> <span data-ttu-id="1c4e6-141">Hello hodnota hash MD5 objektu blob.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-141">hello blob's MD5 hash.</span></span>|  
|`Content-Encoding`|<span data-ttu-id="1c4e6-142">Řetězec</span><span class="sxs-lookup"><span data-stu-id="1c4e6-142">String</span></span>|<span data-ttu-id="1c4e6-143">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-143">Optional.</span></span> <span data-ttu-id="1c4e6-144">Hello obsahu objektu blob kódování.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-144">hello blob's content encoding.</span></span>|  
|`Content-Language`|<span data-ttu-id="1c4e6-145">Řetězec</span><span class="sxs-lookup"><span data-stu-id="1c4e6-145">String</span></span>|<span data-ttu-id="1c4e6-146">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-146">Optional.</span></span> <span data-ttu-id="1c4e6-147">Hello jazyk obsahu objektu blob.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-147">hello blob's content language.</span></span>|  
|`Cache-Control`|<span data-ttu-id="1c4e6-148">Řetězec</span><span class="sxs-lookup"><span data-stu-id="1c4e6-148">String</span></span>|<span data-ttu-id="1c4e6-149">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-149">Optional.</span></span> <span data-ttu-id="1c4e6-150">Hello řetězec ovládací prvek mezipaměti pro objekt blob hello.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-150">hello cache control string for hello blob.</span></span>|  

## <a name="next-steps"></a><span data-ttu-id="1c4e6-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1c4e6-151">Next steps</span></span>

<span data-ttu-id="1c4e6-152">V tématu [umožňuje nastavit vlastnosti objektu blob](/rest/api/storageservices/set-blob-properties), [nastavit Metadata objektu Blob](/rest/api/storageservices/set-blob-metadata), a [vlastnosti a metadata pro nastavení a načítání objektů blob prostředky](/rest/api/storageservices/setting-and-retrieving-properties-and-metadata-for-blob-resources) pro podrobná pravidla o nastavení vlastnosti a metadata objektu blob.</span><span class="sxs-lookup"><span data-stu-id="1c4e6-152">See [Set blob properties](/rest/api/storageservices/set-blob-properties), [Set Blob Metadata](/rest/api/storageservices/set-blob-metadata), and [Setting and retrieving properties and metadata for blob resources](/rest/api/storageservices/setting-and-retrieving-properties-and-metadata-for-blob-resources) for detailed rules about setting blob metadata and properties.</span></span>
