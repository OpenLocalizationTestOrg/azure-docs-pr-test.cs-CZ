---
title: "Nastavení vlastností a metadat pomocí Azure Import/Export | Microsoft Docs"
description: "Zjistěte, jak zadat vlastnosti a metadata nastavení na cílový objektů BLOB při spuštění nástroje Azure Import/Export Příprava jednotky."
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
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 1ba6d157402fae0c7d7bf841d2b4e4f6b1ee1084
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="setting-properties-and-metadata-during-the-import-process"></a><span data-ttu-id="a6aea-103">Nastavení vlastností a metadat během procesu importu</span><span class="sxs-lookup"><span data-stu-id="a6aea-103">Setting properties and metadata during the import process</span></span>

<span data-ttu-id="a6aea-104">Když spustíte nástroj Microsoft Azure Import/Export Příprava jednotky, můžete zadat vlastnosti a metadata nastavení na cílový objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="a6aea-104">When you run the Microsoft Azure Import/Export Tool to prepare your drives, you can specify properties and metadata to be set on the destination blobs.</span></span> <span data-ttu-id="a6aea-105">Postupujte následovně:</span><span class="sxs-lookup"><span data-stu-id="a6aea-105">Follow these steps:</span></span>

1.  <span data-ttu-id="a6aea-106">Pokud chcete nastavit vlastnosti objektů blob, vytvořte textový soubor do místního počítače, který určuje názvy a hodnoty vlastností.</span><span class="sxs-lookup"><span data-stu-id="a6aea-106">To set blob properties, create a text file on your local computer that specifies property names and values.</span></span>
2.  <span data-ttu-id="a6aea-107">Pokud chcete nastavit metadata objektu blob, vytvořte textový soubor do místního počítače, který určuje metadata názvy a hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a6aea-107">To set blob metadata, create a text file on your local computer that specifies metadata names and values.</span></span>
3.  <span data-ttu-id="a6aea-108">Předat úplnou cestu do jedné nebo obou těchto souborů do nástroje Azure Import/Export jako součást `PrepImport` operaci.</span><span class="sxs-lookup"><span data-stu-id="a6aea-108">Pass the full path to one or both of these files to the Azure Import/Export Tool as part of the `PrepImport` operation.</span></span>

> [!NOTE]
>  <span data-ttu-id="a6aea-109">Když zadáte vlastnosti nebo metadata souboru v rámci relace kopírování, jsou pro každý objekt blob, který je naimportováno v rámci této relace kopie nastavit tyto vlastnosti nebo metadata.</span><span class="sxs-lookup"><span data-stu-id="a6aea-109">When you specify a properties or metadata file as part of a copy session, those properties or metadata are set for every blob that is imported as part of that copy session.</span></span> <span data-ttu-id="a6aea-110">Pokud chcete určit jinou sadu vlastnosti nebo metadata pro některé objekty BLOB importována, budete muset vytvořit relaci samostatná kopie různé vlastnosti nebo soubory metadat.</span><span class="sxs-lookup"><span data-stu-id="a6aea-110">If you want to specify a different set of properties or metadata for some of the blobs being imported, you'll need to create a separate copy session with different properties or metadata files.</span></span>

## <a name="specify-blob-properties-in-a-text-file"></a><span data-ttu-id="a6aea-111">Zadejte vlastnosti objektů blob v textovém souboru</span><span class="sxs-lookup"><span data-stu-id="a6aea-111">Specify blob properties in a text file</span></span>

<span data-ttu-id="a6aea-112">K určení vlastností objektu blob, vytvořte místní textový soubor a zahrnují kód XML, který určuje názvy vlastností jako elementy a hodnoty vlastností jako hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a6aea-112">To specify blob properties, create a local text file, and include XML that specifies property names as elements, and property values as values.</span></span> <span data-ttu-id="a6aea-113">Tady je příklad, který určuje některé hodnoty vlastností:</span><span class="sxs-lookup"><span data-stu-id="a6aea-113">Here's an example that specifies some property values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Properties>
    <Content-Type>application/octet-stream</Content-Type>
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>
    <Cache-Control>no-cache</Cache-Control>
</Properties>
```

<span data-ttu-id="a6aea-114">Uložte soubor do místního umístění, jako je `C:\WAImportExport\ImportProperties.txt`.</span><span class="sxs-lookup"><span data-stu-id="a6aea-114">Save the file to a local location like `C:\WAImportExport\ImportProperties.txt`.</span></span>

## <a name="specify-blob-metadata-in-a-text-file"></a><span data-ttu-id="a6aea-115">Zadejte metadata objektu blob do textového souboru</span><span class="sxs-lookup"><span data-stu-id="a6aea-115">Specify blob metadata in a text file</span></span>

<span data-ttu-id="a6aea-116">Podobně zadejte metadata objektu blob, vytvořte místní textový soubor, který určuje názvy metadat jako elementy a metadata hodnoty jako hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a6aea-116">Similarly, to specify blob metadata, create a local text file that specifies metadata names as elements, and metadata values as values.</span></span> <span data-ttu-id="a6aea-117">Tady je příklad, který určuje některé hodnoty metadat:</span><span class="sxs-lookup"><span data-stu-id="a6aea-117">Here's an example that specifies some metadata values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Metadata>
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>
    <DataSetName>SampleData</DataSetName>
    <CreationDate>10/1/2013</CreationDate>
</Metadata>
```

<span data-ttu-id="a6aea-118">Uložte soubor do místního umístění, jako je `C:\WAImportExport\ImportMetadata.txt`.</span><span class="sxs-lookup"><span data-stu-id="a6aea-118">Save the file to a local location like `C:\WAImportExport\ImportMetadata.txt`.</span></span>

## <a name="add-the-path-to-properties-and-metadata-files-in-datasetcsv"></a><span data-ttu-id="a6aea-119">Přidejte cestu k vlastnosti a soubory metadat v dataset.csv</span><span class="sxs-lookup"><span data-stu-id="a6aea-119">Add the path to properties and metadata files in dataset.csv</span></span>

```
BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
H:\Video\,https://mystorageaccount.blob.core.windows.net/video/,BlockBlob,rename,None,H:\mydirectory\properties.xml
H:\Photo\,https://mystorageaccount.blob.core.windows.net/photo/,BlockBlob,rename,None,H:\mydirectory\properties.xml
K:\Temp\FavoriteVideo.ISO,https://mystorageaccount.blob.core.windows.net/favorite/FavoriteVideo.ISO,BlockBlob,rename,None,H:\mydirectory\properties.xml
\\myshare\john\music\,https://mystorageaccount.blob.core.windows.net/music/,BlockBlob,rename,None,H:\mydirectory\properties.xml
```

## <a name="next-steps"></a><span data-ttu-id="a6aea-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a6aea-120">Next steps</span></span>

* [<span data-ttu-id="a6aea-121">Formát souborů metadat a vlastností služby Import/export</span><span class="sxs-lookup"><span data-stu-id="a6aea-121">Import/Export service metadata and properties file format</span></span>](../storage-import-export-file-format-metadata-and-properties.md)
