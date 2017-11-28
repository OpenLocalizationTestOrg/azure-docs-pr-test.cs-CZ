---
title: "aaaSetting vlastnosti a metadat pomocí Azure Import/Export | Microsoft Docs"
description: "Zjistěte, jak nastavit toospecify vlastnosti a metadata toobe na objekty BLOB cílové hello při spuštění nástroje Azure Import/Export tooprepare hello jednotky."
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
ms.openlocfilehash: c763237160f0e4b72ce88fd31e2958994bfe8e50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="setting-properties-and-metadata-during-hello-import-process"></a><span data-ttu-id="02a24-103">Vlastnosti nastavení a metadata během hello importu</span><span class="sxs-lookup"><span data-stu-id="02a24-103">Setting properties and metadata during hello import process</span></span>

<span data-ttu-id="02a24-104">Když spustíte nástroj Microsoft Azure Import/Export tooprepare hello jednotky, můžete zadat vlastnosti a metadata toobe nastavit na hello cílové objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="02a24-104">When you run hello Microsoft Azure Import/Export Tool tooprepare your drives, you can specify properties and metadata toobe set on hello destination blobs.</span></span> <span data-ttu-id="02a24-105">Postupujte následovně:</span><span class="sxs-lookup"><span data-stu-id="02a24-105">Follow these steps:</span></span>

1.  <span data-ttu-id="02a24-106">vlastnosti objektu blob tooset, vytvořte textový soubor do místního počítače, který určuje názvy a hodnoty vlastností.</span><span class="sxs-lookup"><span data-stu-id="02a24-106">tooset blob properties, create a text file on your local computer that specifies property names and values.</span></span>
2.  <span data-ttu-id="02a24-107">tooset metadata objektu blob, vytvořte textový soubor do místního počítače, který určuje metadata názvy a hodnoty.</span><span class="sxs-lookup"><span data-stu-id="02a24-107">tooset blob metadata, create a text file on your local computer that specifies metadata names and values.</span></span>
3.  <span data-ttu-id="02a24-108">Předat tooone hello úplnou cestu nebo oba tyto soubory toohello nástroj Azure Import/Export jako součást hello `PrepImport` operaci.</span><span class="sxs-lookup"><span data-stu-id="02a24-108">Pass hello full path tooone or both of these files toohello Azure Import/Export Tool as part of hello `PrepImport` operation.</span></span>

> [!NOTE]
>  <span data-ttu-id="02a24-109">Když zadáte vlastnosti nebo metadata souboru v rámci relace kopírování, jsou pro každý objekt blob, který je naimportováno v rámci této relace kopie nastavit tyto vlastnosti nebo metadata.</span><span class="sxs-lookup"><span data-stu-id="02a24-109">When you specify a properties or metadata file as part of a copy session, those properties or metadata are set for every blob that is imported as part of that copy session.</span></span> <span data-ttu-id="02a24-110">Pokud chcete pro některé objekty BLOB hello importovaných toospecify jinou sadu vlastnosti nebo metadata, budete potřebovat toocreate samostatné zkopírujte relace s jinou vlastnosti nebo soubory metadat.</span><span class="sxs-lookup"><span data-stu-id="02a24-110">If you want toospecify a different set of properties or metadata for some of hello blobs being imported, you'll need toocreate a separate copy session with different properties or metadata files.</span></span>

## <a name="specify-blob-properties-in-a-text-file"></a><span data-ttu-id="02a24-111">Zadejte vlastnosti objektů blob v textovém souboru</span><span class="sxs-lookup"><span data-stu-id="02a24-111">Specify blob properties in a text file</span></span>

<span data-ttu-id="02a24-112">vlastnosti objektu blob toospecify, vytvořte místní textový soubor a zahrnují kód XML, který určuje názvy vlastností jako elementy a hodnoty vlastností jako hodnoty.</span><span class="sxs-lookup"><span data-stu-id="02a24-112">toospecify blob properties, create a local text file, and include XML that specifies property names as elements, and property values as values.</span></span> <span data-ttu-id="02a24-113">Tady je příklad, který určuje některé hodnoty vlastností:</span><span class="sxs-lookup"><span data-stu-id="02a24-113">Here's an example that specifies some property values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Properties>
    <Content-Type>application/octet-stream</Content-Type>
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>
    <Cache-Control>no-cache</Cache-Control>
</Properties>
```

<span data-ttu-id="02a24-114">Uložit místní umístění tooa hello souboru jako `C:\WAImportExport\ImportProperties.txt`.</span><span class="sxs-lookup"><span data-stu-id="02a24-114">Save hello file tooa local location like `C:\WAImportExport\ImportProperties.txt`.</span></span>

## <a name="specify-blob-metadata-in-a-text-file"></a><span data-ttu-id="02a24-115">Zadejte metadata objektu blob do textového souboru</span><span class="sxs-lookup"><span data-stu-id="02a24-115">Specify blob metadata in a text file</span></span>

<span data-ttu-id="02a24-116">Podobně toospecify metadata objektu blob, vytvořte místní textový soubor, který určuje názvy metadat jako elementy a metadata hodnoty jako hodnoty.</span><span class="sxs-lookup"><span data-stu-id="02a24-116">Similarly, toospecify blob metadata, create a local text file that specifies metadata names as elements, and metadata values as values.</span></span> <span data-ttu-id="02a24-117">Tady je příklad, který určuje některé hodnoty metadat:</span><span class="sxs-lookup"><span data-stu-id="02a24-117">Here's an example that specifies some metadata values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Metadata>
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>
    <DataSetName>SampleData</DataSetName>
    <CreationDate>10/1/2013</CreationDate>
</Metadata>
```

<span data-ttu-id="02a24-118">Uložit místní umístění tooa hello souboru jako `C:\WAImportExport\ImportMetadata.txt`.</span><span class="sxs-lookup"><span data-stu-id="02a24-118">Save hello file tooa local location like `C:\WAImportExport\ImportMetadata.txt`.</span></span>

## <a name="add-hello-path-tooproperties-and-metadata-files-in-datasetcsv"></a><span data-ttu-id="02a24-119">Přidání hello cesta tooproperties a metadata souborů v dataset.csv</span><span class="sxs-lookup"><span data-stu-id="02a24-119">Add hello path tooproperties and metadata files in dataset.csv</span></span>

```
BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
H:\Video\,https://mystorageaccount.blob.core.windows.net/video/,BlockBlob,rename,None,H:\mydirectory\properties.xml
H:\Photo\,https://mystorageaccount.blob.core.windows.net/photo/,BlockBlob,rename,None,H:\mydirectory\properties.xml
K:\Temp\FavoriteVideo.ISO,https://mystorageaccount.blob.core.windows.net/favorite/FavoriteVideo.ISO,BlockBlob,rename,None,H:\mydirectory\properties.xml
\\myshare\john\music\,https://mystorageaccount.blob.core.windows.net/music/,BlockBlob,rename,None,H:\mydirectory\properties.xml
```

## <a name="next-steps"></a><span data-ttu-id="02a24-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="02a24-120">Next steps</span></span>

* [<span data-ttu-id="02a24-121">Formát souborů metadat a vlastností služby Import/export</span><span class="sxs-lookup"><span data-stu-id="02a24-121">Import/Export service metadata and properties file format</span></span>](../storage-import-export-file-format-metadata-and-properties.md)
