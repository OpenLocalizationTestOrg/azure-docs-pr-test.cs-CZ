---
title: "Nastavení vlastností a metadat pomocí Azure Import/Export - v1 | Microsoft Docs"
description: "Zjistěte, jak zadat vlastnosti a metadata nastavení na cílový objektů BLOB při spuštění nástroje Azure Import/Export Příprava jednotky. Vztahuje se k v1 nástroj importu a exportu."
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
ms.openlocfilehash: 6455ce57572f9ec36d0ebae88c1ddd9f40f237bf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="setting-properties-and-metadata-during-the-import-process"></a><span data-ttu-id="f6bea-104">Nastavení vlastností a metadat během procesu importu</span><span class="sxs-lookup"><span data-stu-id="f6bea-104">Setting properties and metadata during the import process</span></span>
<span data-ttu-id="f6bea-105">Když spustíte nástroj Microsoft Azure Import/Export Příprava jednotky, můžete zadat vlastnosti a metadata nastavení na cílový objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="f6bea-105">When you run the Microsoft Azure Import/Export Tool to prepare your drives, you can specify properties and metadata to be set on the destination blobs.</span></span> <span data-ttu-id="f6bea-106">Postupujte následovně:</span><span class="sxs-lookup"><span data-stu-id="f6bea-106">Follow these steps:</span></span>  
  
1.  <span data-ttu-id="f6bea-107">Pokud chcete nastavit vlastnosti objektů blob, vytvořte textový soubor do místního počítače, který určuje názvy a hodnoty vlastností.</span><span class="sxs-lookup"><span data-stu-id="f6bea-107">To set blob properties, create a text file on your local computer that specifies property names and values.</span></span>  
  
2.  <span data-ttu-id="f6bea-108">Pokud chcete nastavit metadata objektu blob, vytvořte textový soubor do místního počítače, který určuje metadata názvy a hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f6bea-108">To set blob metadata, create a text file on your local computer that specifies metadata names and values.</span></span>  
  
3.  <span data-ttu-id="f6bea-109">Předat úplnou cestu do jedné nebo obou těchto souborů do nástroje Azure Import/Export jako součást `PrepImport` operaci.</span><span class="sxs-lookup"><span data-stu-id="f6bea-109">Pass the full path to one or both of these files to the Azure Import/Export Tool as part of the `PrepImport` operation.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="f6bea-110">Když zadáte vlastnosti nebo metadata souboru v rámci relace kopírování, jsou pro každý objekt blob, který je naimportováno v rámci této relace kopie nastavit tyto vlastnosti nebo metadata.</span><span class="sxs-lookup"><span data-stu-id="f6bea-110">When you specify a properties or metadata file as part of a copy session, those properties or metadata are set for every blob that is imported as part of that copy session.</span></span> <span data-ttu-id="f6bea-111">Pokud chcete určit jinou sadu vlastnosti nebo metadata pro některé objekty BLOB importována, budete muset vytvořit relaci samostatná kopie různé vlastnosti nebo soubory metadat.</span><span class="sxs-lookup"><span data-stu-id="f6bea-111">If you want to specify a different set of properties or metadata for some of the blobs being imported, you'll need to create a separate copy session with different properties or metadata files.</span></span>  
  
## <a name="specify-blob-properties-in-a-text-file"></a><span data-ttu-id="f6bea-112">Zadejte vlastnosti objektů Blob v textovém souboru</span><span class="sxs-lookup"><span data-stu-id="f6bea-112">Specify Blob Properties in a Text File</span></span>  
<span data-ttu-id="f6bea-113">K určení vlastností objektu blob, vytvořte místní textový soubor a zahrnují kód XML, který určuje názvy vlastností jako elementy a hodnoty vlastností jako hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f6bea-113">To specify blob properties, create a local text file, and include XML that specifies property names as elements, and property values as values.</span></span> <span data-ttu-id="f6bea-114">Tady je příklad, který určuje některé hodnoty vlastností:</span><span class="sxs-lookup"><span data-stu-id="f6bea-114">Here's an example that specifies some property values:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
    <Content-Type>application/octet-stream</Content-Type>  
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>  
    <Cache-Control>no-cache</Cache-Control>  
</Properties>  
```
  
<span data-ttu-id="f6bea-115">Uložte soubor do místního umístění, jako je `C:\WAImportExport\ImportProperties.txt`.</span><span class="sxs-lookup"><span data-stu-id="f6bea-115">Save the file to a local location like `C:\WAImportExport\ImportProperties.txt`.</span></span>  
  
## <a name="specify-blob-metadata-in-a-text-file"></a><span data-ttu-id="f6bea-116">Zadejte Metadata objektu Blob do textového souboru</span><span class="sxs-lookup"><span data-stu-id="f6bea-116">Specify Blob Metadata in a Text File</span></span>  
<span data-ttu-id="f6bea-117">Podobně zadejte metadata objektu blob, vytvořte místní textový soubor, který určuje názvy metadat jako elementy a metadata hodnoty jako hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f6bea-117">Similarly, to specify blob metadata, create a local text file that specifies metadata names as elements, and metadata values as values.</span></span> <span data-ttu-id="f6bea-118">Tady je příklad, který určuje některé hodnoty metadat:</span><span class="sxs-lookup"><span data-stu-id="f6bea-118">Here's an example that specifies some metadata values:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>  
    <DataSetName>SampleData</DataSetName>  
    <CreationDate>10/1/2013</CreationDate>  
</Metadata>  
```
  
<span data-ttu-id="f6bea-119">Uložte soubor do místního umístění, jako je `C:\WAImportExport\ImportMetadata.txt`.</span><span class="sxs-lookup"><span data-stu-id="f6bea-119">Save the file to a local location like `C:\WAImportExport\ImportMetadata.txt`.</span></span>  
  
## <a name="create-a-copy-session-including-the-properties-or-metadata-files"></a><span data-ttu-id="f6bea-120">Vytvořit relaci kopírování, včetně všech vlastností či soubory metadat</span><span class="sxs-lookup"><span data-stu-id="f6bea-120">Create a Copy Session Including the Properties or Metadata Files</span></span>  
<span data-ttu-id="f6bea-121">Když spustíte nástroj Azure Import/Export připravit úlohu importu, zadejte vlastnosti souboru o použití příkazového řádku `PropertyFile` parametr.</span><span class="sxs-lookup"><span data-stu-id="f6bea-121">When you run the Azure Import/Export Tool to prepare the import job, specify the properties file on the command line using the `PropertyFile` parameter.</span></span> <span data-ttu-id="f6bea-122">Zadejte soubor metadat na pomocí příkazového řádku `/MetadataFile` parametr.</span><span class="sxs-lookup"><span data-stu-id="f6bea-122">Specify the metadata file on the command line using the `/MetadataFile` parameter.</span></span> <span data-ttu-id="f6bea-123">Tady je příklad, který určuje oba soubory:</span><span class="sxs-lookup"><span data-stu-id="f6bea-123">Here's an example that specifies both files:</span></span>  
  
```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:BlueRayIso /srcfile:K:\Temp\BlueRay.ISO /dstblob:favorite/BlueRay.ISO /MetadataFile:c:\WAImportExport\SampleMetadata.txt /PropertyFile:c:\WAImportExport\SampleProperties.txt  
```
  
## <a name="next-steps"></a><span data-ttu-id="f6bea-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f6bea-124">Next steps</span></span>

* [<span data-ttu-id="f6bea-125">Formát souborů metadat a vlastností služby Import/export</span><span class="sxs-lookup"><span data-stu-id="f6bea-125">Import/Export service metadata and properties file format</span></span>](storage-import-export-file-format-metadata-and-properties.md)
