---
title: "aaaSetting vlastnosti a metadat pomocí Azure Import/Export - v1 | Microsoft Docs"
description: "Zjistěte, jak nastavit toospecify vlastnosti a metadata toobe na objekty BLOB cílové hello při spuštění nástroje Azure Import/Export tooprepare hello jednotky. Vztahuje se toov1 hello nástroj pro Import nebo Export."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: e8541695-bcfb-4bf0-84d9-72c9de32a0a8
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 66e55c2076fbcda9b78302f17b5ff2cf96bb24e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="setting-properties-and-metadata-during-hello-import-process"></a><span data-ttu-id="80de3-104">Vlastnosti nastavení a metadata během hello importu</span><span class="sxs-lookup"><span data-stu-id="80de3-104">Setting properties and metadata during hello import process</span></span>
<span data-ttu-id="80de3-105">Když spustíte nástroj Microsoft Azure Import/Export tooprepare hello jednotky, můžete zadat vlastnosti a metadata toobe nastavit na hello cílové objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="80de3-105">When you run hello Microsoft Azure Import/Export Tool tooprepare your drives, you can specify properties and metadata toobe set on hello destination blobs.</span></span> <span data-ttu-id="80de3-106">Postupujte následovně:</span><span class="sxs-lookup"><span data-stu-id="80de3-106">Follow these steps:</span></span>  
  
1.  <span data-ttu-id="80de3-107">vlastnosti objektu blob tooset, vytvořte textový soubor do místního počítače, který určuje názvy a hodnoty vlastností.</span><span class="sxs-lookup"><span data-stu-id="80de3-107">tooset blob properties, create a text file on your local computer that specifies property names and values.</span></span>  
  
2.  <span data-ttu-id="80de3-108">tooset metadata objektu blob, vytvořte textový soubor do místního počítače, který určuje metadata názvy a hodnoty.</span><span class="sxs-lookup"><span data-stu-id="80de3-108">tooset blob metadata, create a text file on your local computer that specifies metadata names and values.</span></span>  
  
3.  <span data-ttu-id="80de3-109">Předat tooone hello úplnou cestu nebo oba tyto soubory toohello nástroj Azure Import/Export jako součást hello `PrepImport` operaci.</span><span class="sxs-lookup"><span data-stu-id="80de3-109">Pass hello full path tooone or both of these files toohello Azure Import/Export Tool as part of hello `PrepImport` operation.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="80de3-110">Když zadáte vlastnosti nebo metadata souboru v rámci relace kopírování, jsou pro každý objekt blob, který je naimportováno v rámci této relace kopie nastavit tyto vlastnosti nebo metadata.</span><span class="sxs-lookup"><span data-stu-id="80de3-110">When you specify a properties or metadata file as part of a copy session, those properties or metadata are set for every blob that is imported as part of that copy session.</span></span> <span data-ttu-id="80de3-111">Pokud chcete pro některé objekty BLOB hello importovaných toospecify jinou sadu vlastnosti nebo metadata, budete potřebovat toocreate samostatné zkopírujte relace s jinou vlastnosti nebo soubory metadat.</span><span class="sxs-lookup"><span data-stu-id="80de3-111">If you want toospecify a different set of properties or metadata for some of hello blobs being imported, you'll need toocreate a separate copy session with different properties or metadata files.</span></span>  
  
## <a name="specify-blob-properties-in-a-text-file"></a><span data-ttu-id="80de3-112">Zadejte vlastnosti objektů Blob v textovém souboru</span><span class="sxs-lookup"><span data-stu-id="80de3-112">Specify Blob Properties in a Text File</span></span>  
<span data-ttu-id="80de3-113">vlastnosti objektu blob toospecify, vytvořte místní textový soubor a zahrnují kód XML, který určuje názvy vlastností jako elementy a hodnoty vlastností jako hodnoty.</span><span class="sxs-lookup"><span data-stu-id="80de3-113">toospecify blob properties, create a local text file, and include XML that specifies property names as elements, and property values as values.</span></span> <span data-ttu-id="80de3-114">Tady je příklad, který určuje některé hodnoty vlastností:</span><span class="sxs-lookup"><span data-stu-id="80de3-114">Here's an example that specifies some property values:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
    <Content-Type>application/octet-stream</Content-Type>  
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>  
    <Cache-Control>no-cache</Cache-Control>  
</Properties>  
```
  
<span data-ttu-id="80de3-115">Uložit místní umístění tooa hello souboru jako `C:\WAImportExport\ImportProperties.txt`.</span><span class="sxs-lookup"><span data-stu-id="80de3-115">Save hello file tooa local location like `C:\WAImportExport\ImportProperties.txt`.</span></span>  
  
## <a name="specify-blob-metadata-in-a-text-file"></a><span data-ttu-id="80de3-116">Zadejte Metadata objektu Blob do textového souboru</span><span class="sxs-lookup"><span data-stu-id="80de3-116">Specify Blob Metadata in a Text File</span></span>  
<span data-ttu-id="80de3-117">Podobně toospecify metadata objektu blob, vytvořte místní textový soubor, který určuje názvy metadat jako elementy a metadata hodnoty jako hodnoty.</span><span class="sxs-lookup"><span data-stu-id="80de3-117">Similarly, toospecify blob metadata, create a local text file that specifies metadata names as elements, and metadata values as values.</span></span> <span data-ttu-id="80de3-118">Tady je příklad, který určuje některé hodnoty metadat:</span><span class="sxs-lookup"><span data-stu-id="80de3-118">Here's an example that specifies some metadata values:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>  
    <DataSetName>SampleData</DataSetName>  
    <CreationDate>10/1/2013</CreationDate>  
</Metadata>  
```
  
<span data-ttu-id="80de3-119">Uložit místní umístění tooa hello souboru jako `C:\WAImportExport\ImportMetadata.txt`.</span><span class="sxs-lookup"><span data-stu-id="80de3-119">Save hello file tooa local location like `C:\WAImportExport\ImportMetadata.txt`.</span></span>  
  
## <a name="create-a-copy-session-including-hello-properties-or-metadata-files"></a><span data-ttu-id="80de3-120">Vytvořit kopie relace včetně hello vlastnosti nebo soubory metadat</span><span class="sxs-lookup"><span data-stu-id="80de3-120">Create a Copy Session Including hello Properties or Metadata Files</span></span>  
<span data-ttu-id="80de3-121">Při spuštění úlohy importu hello nástroj Azure Import/Export tooprepare hello zadejte hello vlastnosti soubor na příkazovém řádku hello pomocí hello `PropertyFile` parametr.</span><span class="sxs-lookup"><span data-stu-id="80de3-121">When you run hello Azure Import/Export Tool tooprepare hello import job, specify hello properties file on hello command line using hello `PropertyFile` parameter.</span></span> <span data-ttu-id="80de3-122">Zadejte soubor metadat hello na hello příkazového řádku pomocí hello `/MetadataFile` parametr.</span><span class="sxs-lookup"><span data-stu-id="80de3-122">Specify hello metadata file on hello command line using hello `/MetadataFile` parameter.</span></span> <span data-ttu-id="80de3-123">Tady je příklad, který určuje oba soubory:</span><span class="sxs-lookup"><span data-stu-id="80de3-123">Here's an example that specifies both files:</span></span>  
  
```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:BlueRayIso /srcfile:K:\Temp\BlueRay.ISO /dstblob:favorite/BlueRay.ISO /MetadataFile:c:\WAImportExport\SampleMetadata.txt /PropertyFile:c:\WAImportExport\SampleProperties.txt  
```
  
## <a name="next-steps"></a><span data-ttu-id="80de3-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="80de3-124">Next steps</span></span>

* [<span data-ttu-id="80de3-125">Formát souborů metadat a vlastností služby Import/export</span><span class="sxs-lookup"><span data-stu-id="80de3-125">Import/Export service metadata and properties file format</span></span>](storage-import-export-file-format-metadata-and-properties.md)
