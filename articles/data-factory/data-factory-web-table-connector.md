---
title: "Přesun dat z tabulky webové pomocí Azure Data Factory | Microsoft Docs"
description: "Další informace o tom, jak přesunout data z tabulky na webové stránce pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: f54a26a4-baa4-4255-9791-5a8f935898e2
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jingwang
ms.openlocfilehash: 9e006bc7289fa0239f1650ac6ad43dd159e3c7e0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="move-data-from-a-web-table-source-using-azure-data-factory"></a><span data-ttu-id="b82fd-103">Přesun dat z webové zdroje tabulky pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="b82fd-103">Move data from a Web table source using Azure Data Factory</span></span>
<span data-ttu-id="b82fd-104">Tento článek popisuje, jak pomocí aktivity kopírování v Azure Data Factory pro přesun dat z tabulky na webové stránce do úložiště dat podporovaných jímky.</span><span class="sxs-lookup"><span data-stu-id="b82fd-104">This article outlines how to use the Copy Activity in Azure Data Factory to move data from a table in a Web page to a supported sink data store.</span></span> <span data-ttu-id="b82fd-105">Tento článek vychází [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který uvádí obecný přehled přesun dat s aktivitou kopírování a seznam úložiště dat, které jsou podporované jako zdroje nebo jímky.</span><span class="sxs-lookup"><span data-stu-id="b82fd-105">This article builds on the [data movement activities](data-factory-data-movement-activities.md) article that presents a general overview of data movement with copy activity and the list of data stores supported as sources/sinks.</span></span>

<span data-ttu-id="b82fd-106">Objekt pro vytváření dat aktuálně podporuje pouze přesunutí dat z tabulky webové k jiným úložištím dat, ale není přesouvání dat od ostatních dat ukládá do cílového umístění v tabulce Web.</span><span class="sxs-lookup"><span data-stu-id="b82fd-106">Data factory currently supports only moving data from a Web table to other data stores, but not moving data from other data stores to a Web table destination.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b82fd-107">Tento konektor webové aktuálně podporuje pouze extrakce obsahu tabulky z stránku HTML.</span><span class="sxs-lookup"><span data-stu-id="b82fd-107">This Web connector currently supports only extracting table content from an HTML page.</span></span> <span data-ttu-id="b82fd-108">Pokud chcete načíst data z koncového bodu protokolu HTTP/s, použijte [HTTP konektor](data-factory-http-connector.md) místo.</span><span class="sxs-lookup"><span data-stu-id="b82fd-108">To retrieve data from a HTTP/s endpoint, use [HTTP connector](data-factory-http-connector.md) instead.</span></span>

## <a name="getting-started"></a><span data-ttu-id="b82fd-109">Začínáme</span><span class="sxs-lookup"><span data-stu-id="b82fd-109">Getting started</span></span>
<span data-ttu-id="b82fd-110">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z úložiště místní Cassandra data pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b82fd-110">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="b82fd-111">Nejjednodušší způsob, jak vytvořit kanál je použití **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="b82fd-111">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="b82fd-112">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírováním data.</span><span class="sxs-lookup"><span data-stu-id="b82fd-112">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span> 
- <span data-ttu-id="b82fd-113">Tyto nástroje můžete také použít k vytvoření kanálu: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru**, **.NET API**, a **REST API**.</span><span class="sxs-lookup"><span data-stu-id="b82fd-113">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="b82fd-114">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="b82fd-114">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="b82fd-115">Jestli používáte nástroje nebo rozhraní API, je třeba provést následující kroky k vytvoření kanálu, který přesouvá data ze zdrojového úložiště dat do úložiště dat podřízený:</span><span class="sxs-lookup"><span data-stu-id="b82fd-115">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="b82fd-116">Vytvoření **propojené služby** propojení vstupní a výstupní data ukládá do data factory.</span><span class="sxs-lookup"><span data-stu-id="b82fd-116">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="b82fd-117">Vytvoření **datové sady** představují vstupní a výstupní data pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="b82fd-117">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="b82fd-118">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="b82fd-118">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="b82fd-119">Když použijete průvodce, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál).</span><span class="sxs-lookup"><span data-stu-id="b82fd-119">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="b82fd-120">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), definujete tyto entity služby Data Factory pomocí formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="b82fd-120">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="b82fd-121">Příklad s definicemi JSON entit služby Data Factory, které se používají ke zkopírování dat z tabulky web, naleznete v tématu [JSON příklad: kopírování dat z tabulky webové do objektu Blob Azure](#json-example-copy-data-from-web-table-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="b82fd-121">For a sample with JSON definitions for Data Factory entities that are used to copy data from a web table, see [JSON example: Copy data from Web table to Azure Blob](#json-example-copy-data-from-web-table-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="b82fd-122">Následující části obsahují podrobnosti o vlastnostech formátu JSON, které slouží k určení entit služby Data Factory konkrétní do tabulky Web:</span><span class="sxs-lookup"><span data-stu-id="b82fd-122">The following sections provide details about JSON properties that are used to define Data Factory entities specific to a Web table:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="b82fd-123">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="b82fd-123">Linked service properties</span></span>
<span data-ttu-id="b82fd-124">Následující tabulka obsahuje popis JSON elementy, které jsou specifické pro webové propojené služby.</span><span class="sxs-lookup"><span data-stu-id="b82fd-124">The following table provides description for JSON elements specific to Web linked service.</span></span>

| <span data-ttu-id="b82fd-125">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="b82fd-125">Property</span></span> | <span data-ttu-id="b82fd-126">Popis</span><span class="sxs-lookup"><span data-stu-id="b82fd-126">Description</span></span> | <span data-ttu-id="b82fd-127">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b82fd-127">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b82fd-128">type</span><span class="sxs-lookup"><span data-stu-id="b82fd-128">type</span></span> |<span data-ttu-id="b82fd-129">Vlastnost typu musí být nastavena na: **Web**</span><span class="sxs-lookup"><span data-stu-id="b82fd-129">The type property must be set to: **Web**</span></span> |<span data-ttu-id="b82fd-130">Ano</span><span class="sxs-lookup"><span data-stu-id="b82fd-130">Yes</span></span> |
| <span data-ttu-id="b82fd-131">URL</span><span class="sxs-lookup"><span data-stu-id="b82fd-131">Url</span></span> |<span data-ttu-id="b82fd-132">Adresa URL pro webové zdroje</span><span class="sxs-lookup"><span data-stu-id="b82fd-132">URL to the Web source</span></span> |<span data-ttu-id="b82fd-133">Ano</span><span class="sxs-lookup"><span data-stu-id="b82fd-133">Yes</span></span> |
| <span data-ttu-id="b82fd-134">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="b82fd-134">authenticationType</span></span> |<span data-ttu-id="b82fd-135">Anonymní.</span><span class="sxs-lookup"><span data-stu-id="b82fd-135">Anonymous.</span></span> |<span data-ttu-id="b82fd-136">Ano</span><span class="sxs-lookup"><span data-stu-id="b82fd-136">Yes</span></span> |

### <a name="using-anonymous-authentication"></a><span data-ttu-id="b82fd-137">Pomocí anonymní ověřování</span><span class="sxs-lookup"><span data-stu-id="b82fd-137">Using Anonymous authentication</span></span>

```json
{
    "name": "web",
    "properties":
    {
        "type": "Web",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "url" : "https://en.wikipedia.org/wiki/"
        }
    }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="b82fd-138">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="b82fd-138">Dataset properties</span></span>
<span data-ttu-id="b82fd-139">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v článku [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="b82fd-139">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="b82fd-140">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="b82fd-140">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="b82fd-141">**Rámci typeProperties** oddílu se liší pro jednotlivé typy datovou sadu a informace o umístění dat v úložišti dat.</span><span class="sxs-lookup"><span data-stu-id="b82fd-141">The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="b82fd-142">Rámci typeProperties část datové sady typ **WebTable** má následující vlastnosti</span><span class="sxs-lookup"><span data-stu-id="b82fd-142">The typeProperties section for dataset of type **WebTable** has the following properties</span></span>

| <span data-ttu-id="b82fd-143">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="b82fd-143">Property</span></span> | <span data-ttu-id="b82fd-144">Popis</span><span class="sxs-lookup"><span data-stu-id="b82fd-144">Description</span></span> | <span data-ttu-id="b82fd-145">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b82fd-145">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="b82fd-146">type</span><span class="sxs-lookup"><span data-stu-id="b82fd-146">type</span></span> |<span data-ttu-id="b82fd-147">Typ datové sady.</span><span class="sxs-lookup"><span data-stu-id="b82fd-147">type of the dataset.</span></span> <span data-ttu-id="b82fd-148">musí být nastavena na **WebTable**</span><span class="sxs-lookup"><span data-stu-id="b82fd-148">must be set to **WebTable**</span></span> |<span data-ttu-id="b82fd-149">Ano</span><span class="sxs-lookup"><span data-stu-id="b82fd-149">Yes</span></span> |
| <span data-ttu-id="b82fd-150">Cesta</span><span class="sxs-lookup"><span data-stu-id="b82fd-150">path</span></span> |<span data-ttu-id="b82fd-151">Relativní adresa URL prostředek, který obsahuje tabulku.</span><span class="sxs-lookup"><span data-stu-id="b82fd-151">A relative URL to the resource that contains the table.</span></span> |<span data-ttu-id="b82fd-152">Ne.</span><span class="sxs-lookup"><span data-stu-id="b82fd-152">No.</span></span> <span data-ttu-id="b82fd-153">Pokud cesta není zadána, je použít jenom adresu URL, zadaný v definici propojené služby.</span><span class="sxs-lookup"><span data-stu-id="b82fd-153">When path is not specified, only the URL specified in the linked service definition is used.</span></span> |
| <span data-ttu-id="b82fd-154">Index</span><span class="sxs-lookup"><span data-stu-id="b82fd-154">index</span></span> |<span data-ttu-id="b82fd-155">Index tabulky v prostředku.</span><span class="sxs-lookup"><span data-stu-id="b82fd-155">The index of the table in the resource.</span></span> <span data-ttu-id="b82fd-156">V tématu [Get index tabulky v stránku HTML](#get-index-of-a-table-in-an-html-page) části Postup získání index tabulky v stránku HTML.</span><span class="sxs-lookup"><span data-stu-id="b82fd-156">See [Get index of a table in an HTML page](#get-index-of-a-table-in-an-html-page) section for steps to getting index of a table in an HTML page.</span></span> |<span data-ttu-id="b82fd-157">Ano</span><span class="sxs-lookup"><span data-stu-id="b82fd-157">Yes</span></span> |

<span data-ttu-id="b82fd-158">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="b82fd-158">**Example:**</span></span>

```json
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": "WebLinkedService",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

## <a name="copy-activity-properties"></a><span data-ttu-id="b82fd-159">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="b82fd-159">Copy activity properties</span></span>
<span data-ttu-id="b82fd-160">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="b82fd-160">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="b82fd-161">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="b82fd-161">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="b82fd-162">Vzhledem k tomu, vlastnosti dostupné v rámci typeProperties části aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="b82fd-162">Whereas, properties available in the typeProperties section of the activity vary with each activity type.</span></span> <span data-ttu-id="b82fd-163">Pro aktivitu kopírování budou lišit v závislosti na typech zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="b82fd-163">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="b82fd-164">V současné době po zdroji v aktivitě kopírování typu **WebSource**, jsou podporovány žádné další vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="b82fd-164">Currently, when the source in copy activity is of type **WebSource**, no additional properties are supported.</span></span>


## <a name="json-example-copy-data-from-web-table-to-azure-blob"></a><span data-ttu-id="b82fd-165">Příklad JSON: kopírování dat z tabulky webové do objektu Blob Azure</span><span class="sxs-lookup"><span data-stu-id="b82fd-165">JSON example: Copy data from Web table to Azure Blob</span></span>
<span data-ttu-id="b82fd-166">Následující příklad ukazuje:</span><span class="sxs-lookup"><span data-stu-id="b82fd-166">The following sample shows:</span></span>

1. <span data-ttu-id="b82fd-167">Propojené služby typu [webové](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="b82fd-167">A linked service of type [Web](#linked-service-properties).</span></span>
2. <span data-ttu-id="b82fd-168">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="b82fd-168">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="b82fd-169">Vstup [datovou sadu](data-factory-create-datasets.md) typu [WebTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="b82fd-169">An input [dataset](data-factory-create-datasets.md) of type [WebTable](#dataset-properties).</span></span>
4. <span data-ttu-id="b82fd-170">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="b82fd-170">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="b82fd-171">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [WebSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="b82fd-171">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [WebSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="b82fd-172">Ukázka zkopíruje data z tabulky webové do objektu blob Azure každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="b82fd-172">The sample copies data from a Web table to an Azure blob every hour.</span></span> <span data-ttu-id="b82fd-173">Vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="b82fd-173">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="b82fd-174">Následující příklad ukazuje, jak zkopírovat data z tabulky webové do objektu blob Azure.</span><span class="sxs-lookup"><span data-stu-id="b82fd-174">The following sample shows how to copy data from a Web table to an Azure blob.</span></span> <span data-ttu-id="b82fd-175">Ale data se dají zkopírovat přímo do jakéhokoli z jímky uvádí [aktivity přesunu dat](data-factory-data-movement-activities.md) článek pomocí aktivity kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="b82fd-175">However, data can be copied directly to any of the sinks stated in the [Data Movement Activities](data-factory-data-movement-activities.md) article by using the Copy Activity in Azure Data Factory.</span></span>

<span data-ttu-id="b82fd-176">**Webové propojená služba** tento příklad používá webové propojené služby s anonymní ověřování.</span><span class="sxs-lookup"><span data-stu-id="b82fd-176">**Web linked service** This example uses the Web linked service with anonymous authentication.</span></span> <span data-ttu-id="b82fd-177">V tématu [webové propojená služba](#linked-service-properties) části pro různé typy ověřování můžete použít.</span><span class="sxs-lookup"><span data-stu-id="b82fd-177">See [Web linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

```json
{
    "name": "WebLinkedService",
    "properties":
    {
        "type": "Web",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "url" : "https://en.wikipedia.org/wiki/"
        }
    }
}
```

<span data-ttu-id="b82fd-178">**Propojená služba Azure Storage**</span><span class="sxs-lookup"><span data-stu-id="b82fd-178">**Azure Storage linked service**</span></span>

```json
{
  "name": "AzureStorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```

<span data-ttu-id="b82fd-179">**Vstupní datové sady WebTable** nastavení **externí** k **true** služba Data Factory informuje, že datová sada je externí k objektu pro vytváření dat a není vyprodukované aktivitu v datech objekt pro vytváření.</span><span class="sxs-lookup"><span data-stu-id="b82fd-179">**WebTable input dataset** Setting **external** to **true** informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

> [!NOTE]
> <span data-ttu-id="b82fd-180">V tématu [Get index tabulky v stránku HTML](#get-index-of-a-table-in-an-html-page) části Postup získání index tabulky v stránku HTML.</span><span class="sxs-lookup"><span data-stu-id="b82fd-180">See [Get index of a table in an HTML page](#get-index-of-a-table-in-an-html-page) section for steps to getting index of a table in an HTML page.</span></span>  
>
>

```json
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": "WebLinkedService",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```


<span data-ttu-id="b82fd-181">**Výstupní datovou sadu objektů Blob v Azure**</span><span class="sxs-lookup"><span data-stu-id="b82fd-181">**Azure Blob output dataset**</span></span>

<span data-ttu-id="b82fd-182">Data se zapisují do nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="b82fd-182">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span>

```json
{
    "name": "AzureBlobOutput",
    "properties":
    {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties":
        {
            "folderPath": "adfgetstarted/Movies"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```



<span data-ttu-id="b82fd-183">**Kanál s aktivitou kopírování**</span><span class="sxs-lookup"><span data-stu-id="b82fd-183">**Pipeline with Copy activity**</span></span>

<span data-ttu-id="b82fd-184">Kanál obsahuje aktivitu kopírování, který je nakonfigurovaný na použití vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="b82fd-184">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="b82fd-185">V definici JSON kanálu **zdroj** je typ nastaven na **WebSource** a **podřízený** je typ nastaven na **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="b82fd-185">In the pipeline JSON definition, the **source** type is set to **WebSource** and **sink** type is set to **BlobSink**.</span></span>

<span data-ttu-id="b82fd-186">V tématu [vlastnosti typu WebSource](#copy-activity-type-properties) pro seznam vlastností WebSource nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="b82fd-186">See [WebSource type properties](#copy-activity-type-properties) for the list of properties supported by the WebSource.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "WebTableToAzureBlob",
        "description": "Copy from a Web table to an Azure blob",
        "type": "Copy",
        "inputs": [
          {
            "name": "WebTableInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "WebSource"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
      ]
   }
}
```

## <a name="get-index-of-a-table-in-an-html-page"></a><span data-ttu-id="b82fd-187">Na stránce HTML získat index tabulky.</span><span class="sxs-lookup"><span data-stu-id="b82fd-187">Get index of a table in an HTML page</span></span>
1. <span data-ttu-id="b82fd-188">Spusťte **Excel 2016** a přepněte do **Data** kartě.</span><span class="sxs-lookup"><span data-stu-id="b82fd-188">Launch **Excel 2016** and switch to the **Data** tab.</span></span>  
2. <span data-ttu-id="b82fd-189">Klikněte na tlačítko **nový dotaz** na panelu nástrojů, přejděte na **z jiných zdrojů** a klikněte na tlačítko **z webové**.</span><span class="sxs-lookup"><span data-stu-id="b82fd-189">Click **New Query** on the toolbar, point to **From Other Sources** and click **From Web**.</span></span>

    ![Power Query nabídky](./media/data-factory-web-table-connector/PowerQuery-Menu.png)
3. <span data-ttu-id="b82fd-191">V **z webové** dialogovém okně zadejte **URL** , kterou použijete v propojené službě JSON (například: https://en.wikipedia.org/wiki/) společně s cesty zadáte pro datovou sadu (například: AFI % 27s_ 100_Years... 100_Movies) a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="b82fd-191">In the **From Web** dialog box, enter **URL** that you would use in linked service JSON (for example: https://en.wikipedia.org/wiki/) along with path you would specify for the dataset (for example: AFI%27s_100_Years...100_Movies), and click **OK**.</span></span>

    ![Z webové dialogového okna](./media/data-factory-web-table-connector/FromWeb-DialogBox.png)

    <span data-ttu-id="b82fd-193">Adresa URL použitá v tomto příkladu: https://en.wikipedia.org/wiki/AFI%27s_100_Years...100_Movies</span><span class="sxs-lookup"><span data-stu-id="b82fd-193">URL used in this example: https://en.wikipedia.org/wiki/AFI%27s_100_Years...100_Movies</span></span>
4. <span data-ttu-id="b82fd-194">Pokud se zobrazí **přístup k webovému obsahu** dialogovém okně vyberte právo **adresa URL**, **ověřování**a klikněte na tlačítko **Connect**.</span><span class="sxs-lookup"><span data-stu-id="b82fd-194">If you see **Access Web content** dialog box, select the right **URL**, **authentication**, and click **Connect**.</span></span>

   ![Přístup k dialogové okno webového obsahu](./media/data-factory-web-table-connector/AccessWebContentDialog.png)
5. <span data-ttu-id="b82fd-196">Klikněte na tlačítko **tabulky** položky ve stromovém zobrazení zobrazit obsah z tabulky a klikněte na tlačítko **upravit** tlačítko dole.</span><span class="sxs-lookup"><span data-stu-id="b82fd-196">Click a **table** item in the tree view to see content from the table and then click **Edit** button at the bottom.</span></span>  

   ![Dialogové okno Navigátor](./media/data-factory-web-table-connector/Navigator-DialogBox.png)
6. <span data-ttu-id="b82fd-198">V **Editor dotazů** okně klikněte na tlačítko **Upřesnit** tlačítka na panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="b82fd-198">In the **Query Editor** window, click **Advanced Editor** button on the toolbar.</span></span>

    ![Tlačítko Rozšířené editoru](./media/data-factory-web-table-connector/QueryEditor-AdvancedEditorButton.png)
7. <span data-ttu-id="b82fd-200">V dialogovém okně Upřesnit číslo vedle "Zdroj" je index.</span><span class="sxs-lookup"><span data-stu-id="b82fd-200">In the Advanced Editor dialog box, the number next to "Source" is the index.</span></span>

    ![Rozšířené Editor - indexu](./media/data-factory-web-table-connector/AdvancedEditor-Index.png)

<span data-ttu-id="b82fd-202">Pokud používáte Excel 2013, použijte [Microsoft Power Query pro Excel](https://www.microsoft.com/download/details.aspx?id=39379) získat index.</span><span class="sxs-lookup"><span data-stu-id="b82fd-202">If you are using Excel 2013, use [Microsoft Power Query for Excel](https://www.microsoft.com/download/details.aspx?id=39379) to get the index.</span></span> <span data-ttu-id="b82fd-203">V tématu [připojit k webové stránce](https://support.office.com/article/Connect-to-a-web-page-Power-Query-b2725d67-c9e8-43e6-a590-c0a175bd64d8) článku.</span><span class="sxs-lookup"><span data-stu-id="b82fd-203">See [Connect to a web page](https://support.office.com/article/Connect-to-a-web-page-Power-Query-b2725d67-c9e8-43e6-a590-c0a175bd64d8) article for details.</span></span> <span data-ttu-id="b82fd-204">Kroky jsou podobné, pokud používáte [Microsoft Power BI pro plochu](https://powerbi.microsoft.com/desktop/).</span><span class="sxs-lookup"><span data-stu-id="b82fd-204">The steps are similar if you are using [Microsoft Power BI for Desktop](https://powerbi.microsoft.com/desktop/).</span></span>

> [!NOTE]
> <span data-ttu-id="b82fd-205">Mapování sloupců z datové sady zdroje na sloupce ze sady jímku dat naleznete v tématu [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="b82fd-205">To map columns from source dataset to columns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="b82fd-206">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="b82fd-206">Performance and Tuning</span></span>
<span data-ttu-id="b82fd-207">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) Další informace o klíčových faktorů, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby, jak optimalizovat ho.</span><span class="sxs-lookup"><span data-stu-id="b82fd-207">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
