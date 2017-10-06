---
title: "aaaMove data z tabulky webové pomocí Azure Data Factory | Microsoft Docs"
description: "Informace o tom, jak toomove data z tabulky v webové stránce pomocí Azure Data Factory."
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
ms.openlocfilehash: e52216305583ebbe71ed896522f361bb22f01278
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-a-web-table-source-using-azure-data-factory"></a><span data-ttu-id="14215-103">Přesun dat z webové zdroje tabulky pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="14215-103">Move data from a Web table source using Azure Data Factory</span></span>
<span data-ttu-id="14215-104">Tento článek popisuje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove data z tabulky v tooa webové stránky podporované úložiště dat jímky.</span><span class="sxs-lookup"><span data-stu-id="14215-104">This article outlines how toouse hello Copy Activity in Azure Data Factory toomove data from a table in a Web page tooa supported sink data store.</span></span> <span data-ttu-id="14215-105">Tento článek vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s kopie aktivity a hello seznamu úložiště dat, které jsou podporované jako zdroje nebo jímky.</span><span class="sxs-lookup"><span data-stu-id="14215-105">This article builds on hello [data movement activities](data-factory-data-movement-activities.md) article that presents a general overview of data movement with copy activity and hello list of data stores supported as sources/sinks.</span></span>

<span data-ttu-id="14215-106">Objekt pro vytváření dat aktuálně podporuje pouze přesunutí dat z datové webové tabulky tooother ukládá, ale není přesouvání dat od ostatních dat ukládá cíl tabulky tooa webu.</span><span class="sxs-lookup"><span data-stu-id="14215-106">Data factory currently supports only moving data from a Web table tooother data stores, but not moving data from other data stores tooa Web table destination.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="14215-107">Tento konektor webové aktuálně podporuje pouze extrakce obsahu tabulky z stránku HTML.</span><span class="sxs-lookup"><span data-stu-id="14215-107">This Web connector currently supports only extracting table content from an HTML page.</span></span> <span data-ttu-id="14215-108">tooretrieve data z koncového bodu protokolu HTTP/s, používat [HTTP konektor](data-factory-http-connector.md) místo.</span><span class="sxs-lookup"><span data-stu-id="14215-108">tooretrieve data from a HTTP/s endpoint, use [HTTP connector](data-factory-http-connector.md) instead.</span></span>

## <a name="getting-started"></a><span data-ttu-id="14215-109">Začínáme</span><span class="sxs-lookup"><span data-stu-id="14215-109">Getting started</span></span>
<span data-ttu-id="14215-110">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z úložiště místní Cassandra data pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="14215-110">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="14215-111">Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="14215-111">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="14215-112">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.</span><span class="sxs-lookup"><span data-stu-id="14215-112">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span> 
- <span data-ttu-id="14215-113">Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**.</span><span class="sxs-lookup"><span data-stu-id="14215-113">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="14215-114">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="14215-114">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="14215-115">Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:</span><span class="sxs-lookup"><span data-stu-id="14215-115">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="14215-116">Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="14215-116">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="14215-117">Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="14215-117">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="14215-118">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="14215-118">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="14215-119">Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello).</span><span class="sxs-lookup"><span data-stu-id="14215-119">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="14215-120">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.</span><span class="sxs-lookup"><span data-stu-id="14215-120">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="14215-121">Příklad s definicemi JSON entit služby Data Factory, které jsou používané toocopy data z tabulky web, naleznete v tématu [JSON příklad: kopírování dat z webové tabulky tooAzure Blob](#json-example-copy-data-from-web-table-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="14215-121">For a sample with JSON definitions for Data Factory entities that are used toocopy data from a web table, see [JSON example: Copy data from Web table tooAzure Blob](#json-example-copy-data-from-web-table-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="14215-122">Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooa webové tabulce:</span><span class="sxs-lookup"><span data-stu-id="14215-122">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooa Web table:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="14215-123">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="14215-123">Linked service properties</span></span>
<span data-ttu-id="14215-124">Hello následující tabulka obsahuje popis JSON elementy konkrétní tooWeb propojené služby.</span><span class="sxs-lookup"><span data-stu-id="14215-124">hello following table provides description for JSON elements specific tooWeb linked service.</span></span>

| <span data-ttu-id="14215-125">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="14215-125">Property</span></span> | <span data-ttu-id="14215-126">Popis</span><span class="sxs-lookup"><span data-stu-id="14215-126">Description</span></span> | <span data-ttu-id="14215-127">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="14215-127">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="14215-128">type</span><span class="sxs-lookup"><span data-stu-id="14215-128">type</span></span> |<span data-ttu-id="14215-129">vlastnost typu Hello musí být nastavena na: **Web**</span><span class="sxs-lookup"><span data-stu-id="14215-129">hello type property must be set to: **Web**</span></span> |<span data-ttu-id="14215-130">Ano</span><span class="sxs-lookup"><span data-stu-id="14215-130">Yes</span></span> |
| <span data-ttu-id="14215-131">URL</span><span class="sxs-lookup"><span data-stu-id="14215-131">Url</span></span> |<span data-ttu-id="14215-132">Adresa URL toohello webové zdroje</span><span class="sxs-lookup"><span data-stu-id="14215-132">URL toohello Web source</span></span> |<span data-ttu-id="14215-133">Ano</span><span class="sxs-lookup"><span data-stu-id="14215-133">Yes</span></span> |
| <span data-ttu-id="14215-134">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="14215-134">authenticationType</span></span> |<span data-ttu-id="14215-135">Anonymní.</span><span class="sxs-lookup"><span data-stu-id="14215-135">Anonymous.</span></span> |<span data-ttu-id="14215-136">Ano</span><span class="sxs-lookup"><span data-stu-id="14215-136">Yes</span></span> |

### <a name="using-anonymous-authentication"></a><span data-ttu-id="14215-137">Pomocí anonymní ověřování</span><span class="sxs-lookup"><span data-stu-id="14215-137">Using Anonymous authentication</span></span>

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

## <a name="dataset-properties"></a><span data-ttu-id="14215-138">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="14215-138">Dataset properties</span></span>
<span data-ttu-id="14215-139">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="14215-139">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="14215-140">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="14215-140">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="14215-141">Hello **rámci typeProperties** části se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello.</span><span class="sxs-lookup"><span data-stu-id="14215-141">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="14215-142">rámci typeProperties Hello část datové sady typ **WebTable** má následující vlastnosti hello</span><span class="sxs-lookup"><span data-stu-id="14215-142">hello typeProperties section for dataset of type **WebTable** has hello following properties</span></span>

| <span data-ttu-id="14215-143">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="14215-143">Property</span></span> | <span data-ttu-id="14215-144">Popis</span><span class="sxs-lookup"><span data-stu-id="14215-144">Description</span></span> | <span data-ttu-id="14215-145">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="14215-145">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="14215-146">type</span><span class="sxs-lookup"><span data-stu-id="14215-146">type</span></span> |<span data-ttu-id="14215-147">Typ hello datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="14215-147">type of hello dataset.</span></span> <span data-ttu-id="14215-148">musí být nastaven příliš**WebTable**</span><span class="sxs-lookup"><span data-stu-id="14215-148">must be set too**WebTable**</span></span> |<span data-ttu-id="14215-149">Ano</span><span class="sxs-lookup"><span data-stu-id="14215-149">Yes</span></span> |
| <span data-ttu-id="14215-150">Cesta</span><span class="sxs-lookup"><span data-stu-id="14215-150">path</span></span> |<span data-ttu-id="14215-151">Na relativní adresa URL toohello prostředek, který obsahuje tabulku hello.</span><span class="sxs-lookup"><span data-stu-id="14215-151">A relative URL toohello resource that contains hello table.</span></span> |<span data-ttu-id="14215-152">Ne.</span><span class="sxs-lookup"><span data-stu-id="14215-152">No.</span></span> <span data-ttu-id="14215-153">Pokud cesta není zadána, je použít jenom hello adrese URL zadané v definici hello propojené služby.</span><span class="sxs-lookup"><span data-stu-id="14215-153">When path is not specified, only hello URL specified in hello linked service definition is used.</span></span> |
| <span data-ttu-id="14215-154">Index</span><span class="sxs-lookup"><span data-stu-id="14215-154">index</span></span> |<span data-ttu-id="14215-155">index Hello hello tabulky v hello prostředku.</span><span class="sxs-lookup"><span data-stu-id="14215-155">hello index of hello table in hello resource.</span></span> <span data-ttu-id="14215-156">V tématu [Get index tabulky v stránku HTML](#get-index-of-a-table-in-an-html-page) části kroky toogetting index tabulky v stránku HTML.</span><span class="sxs-lookup"><span data-stu-id="14215-156">See [Get index of a table in an HTML page](#get-index-of-a-table-in-an-html-page) section for steps toogetting index of a table in an HTML page.</span></span> |<span data-ttu-id="14215-157">Ano</span><span class="sxs-lookup"><span data-stu-id="14215-157">Yes</span></span> |

<span data-ttu-id="14215-158">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="14215-158">**Example:**</span></span>

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

## <a name="copy-activity-properties"></a><span data-ttu-id="14215-159">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="14215-159">Copy activity properties</span></span>
<span data-ttu-id="14215-160">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="14215-160">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="14215-161">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="14215-161">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="14215-162">Vzhledem k tomu, vlastnosti dostupné v rámci typeProperties části hello hello aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="14215-162">Whereas, properties available in hello typeProperties section of hello activity vary with each activity type.</span></span> <span data-ttu-id="14215-163">Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="14215-163">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="14215-164">V současné době po hello zdroj v aktivitě kopírování typu **WebSource**, jsou podporovány žádné další vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="14215-164">Currently, when hello source in copy activity is of type **WebSource**, no additional properties are supported.</span></span>


## <a name="json-example-copy-data-from-web-table-tooazure-blob"></a><span data-ttu-id="14215-165">Příklad JSON: kopírování dat z webové tabulky tooAzure objektů Blob</span><span class="sxs-lookup"><span data-stu-id="14215-165">JSON example: Copy data from Web table tooAzure Blob</span></span>
<span data-ttu-id="14215-166">Následující ukázka ukazuje Hello:</span><span class="sxs-lookup"><span data-stu-id="14215-166">hello following sample shows:</span></span>

1. <span data-ttu-id="14215-167">Propojené služby typu [webové](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="14215-167">A linked service of type [Web](#linked-service-properties).</span></span>
2. <span data-ttu-id="14215-168">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="14215-168">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="14215-169">Vstup [datovou sadu](data-factory-create-datasets.md) typu [WebTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="14215-169">An input [dataset](data-factory-create-datasets.md) of type [WebTable](#dataset-properties).</span></span>
4. <span data-ttu-id="14215-170">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="14215-170">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="14215-171">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [WebSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="14215-171">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [WebSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="14215-172">Ukázka Hello zkopíruje data z tabulky tooan webové objektů blob v Azure každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="14215-172">hello sample copies data from a Web table tooan Azure blob every hour.</span></span> <span data-ttu-id="14215-173">Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.</span><span class="sxs-lookup"><span data-stu-id="14215-173">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="14215-174">Následující ukázka Hello ukazuje, jak toocopy dat z tabulky tooan Web Azure blob.</span><span class="sxs-lookup"><span data-stu-id="14215-174">hello following sample shows how toocopy data from a Web table tooan Azure blob.</span></span> <span data-ttu-id="14215-175">Ale data se dají zkopírovat přímo tooany hello jímky stanovené v hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek pomocí hello aktivitu kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="14215-175">However, data can be copied directly tooany of hello sinks stated in hello [Data Movement Activities](data-factory-data-movement-activities.md) article by using hello Copy Activity in Azure Data Factory.</span></span>

<span data-ttu-id="14215-176">**Webové propojená služba** tento příklad používá hello webové propojená služba s anonymní ověřování.</span><span class="sxs-lookup"><span data-stu-id="14215-176">**Web linked service** This example uses hello Web linked service with anonymous authentication.</span></span> <span data-ttu-id="14215-177">V tématu [webové propojená služba](#linked-service-properties) části pro různé typy ověřování můžete použít.</span><span class="sxs-lookup"><span data-stu-id="14215-177">See [Web linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

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

<span data-ttu-id="14215-178">**Propojená služba Azure Storage**</span><span class="sxs-lookup"><span data-stu-id="14215-178">**Azure Storage linked service**</span></span>

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

<span data-ttu-id="14215-179">**Vstupní datové sady WebTable** nastavení **externí** příliš**true** informuje služba Data Factory hello tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v hello služby data factory.</span><span class="sxs-lookup"><span data-stu-id="14215-179">**WebTable input dataset** Setting **external** too**true** informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

> [!NOTE]
> <span data-ttu-id="14215-180">V tématu [Get index tabulky v stránku HTML](#get-index-of-a-table-in-an-html-page) části kroky toogetting index tabulky v stránku HTML.</span><span class="sxs-lookup"><span data-stu-id="14215-180">See [Get index of a table in an HTML page](#get-index-of-a-table-in-an-html-page) section for steps toogetting index of a table in an HTML page.</span></span>  
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


<span data-ttu-id="14215-181">**Výstupní datovou sadu objektů Blob v Azure**</span><span class="sxs-lookup"><span data-stu-id="14215-181">**Azure Blob output dataset**</span></span>

<span data-ttu-id="14215-182">Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="14215-182">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span>

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



<span data-ttu-id="14215-183">**Kanál s aktivitou kopírování**</span><span class="sxs-lookup"><span data-stu-id="14215-183">**Pipeline with Copy activity**</span></span>

<span data-ttu-id="14215-184">Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="14215-184">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="14215-185">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**WebSource** a **podřízený** je typ nastaven příliš**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="14215-185">In hello pipeline JSON definition, hello **source** type is set too**WebSource** and **sink** type is set too**BlobSink**.</span></span>

<span data-ttu-id="14215-186">V tématu [vlastnosti typu WebSource](#copy-activity-type-properties) hello seznam vlastnostech podporovaných zprostředkovatelem hello WebSource.</span><span class="sxs-lookup"><span data-stu-id="14215-186">See [WebSource type properties](#copy-activity-type-properties) for hello list of properties supported by hello WebSource.</span></span>

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
        "description": "Copy from a Web table tooan Azure blob",
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

## <a name="get-index-of-a-table-in-an-html-page"></a><span data-ttu-id="14215-187">Na stránce HTML získat index tabulky.</span><span class="sxs-lookup"><span data-stu-id="14215-187">Get index of a table in an HTML page</span></span>
1. <span data-ttu-id="14215-188">Spusťte **Excel 2016** a přepínač toohello **Data** kartě.</span><span class="sxs-lookup"><span data-stu-id="14215-188">Launch **Excel 2016** and switch toohello **Data** tab.</span></span>  
2. <span data-ttu-id="14215-189">Klikněte na tlačítko **nový dotaz** na panelu nástrojů hello bodu příliš**z jiných zdrojů** a klikněte na tlačítko **z webové**.</span><span class="sxs-lookup"><span data-stu-id="14215-189">Click **New Query** on hello toolbar, point too**From Other Sources** and click **From Web**.</span></span>

    ![Power Query nabídky](./media/data-factory-web-table-connector/PowerQuery-Menu.png)
3. <span data-ttu-id="14215-191">V hello **z webové** dialogovém okně zadejte **URL** , kterou použijete v propojené službě JSON (například: https://en.wikipedia.org/wiki/) společně s cesty zadáte pro datovou sadu hello (například: AFI % 27s_100_Years... 100_Movies) a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="14215-191">In hello **From Web** dialog box, enter **URL** that you would use in linked service JSON (for example: https://en.wikipedia.org/wiki/) along with path you would specify for hello dataset (for example: AFI%27s_100_Years...100_Movies), and click **OK**.</span></span>

    ![Z webové dialogového okna](./media/data-factory-web-table-connector/FromWeb-DialogBox.png)

    <span data-ttu-id="14215-193">Adresa URL použitá v tomto příkladu: https://en.wikipedia.org/wiki/AFI%27s_100_Years...100_Movies</span><span class="sxs-lookup"><span data-stu-id="14215-193">URL used in this example: https://en.wikipedia.org/wiki/AFI%27s_100_Years...100_Movies</span></span>
4. <span data-ttu-id="14215-194">Pokud se zobrazí **přístup k webovému obsahu** dialogové okno, vyberte hello vpravo **URL**, **ověřování**a klikněte na tlačítko **Connect**.</span><span class="sxs-lookup"><span data-stu-id="14215-194">If you see **Access Web content** dialog box, select hello right **URL**, **authentication**, and click **Connect**.</span></span>

   ![Přístup k dialogové okno webového obsahu](./media/data-factory-web-table-connector/AccessWebContentDialog.png)
5. <span data-ttu-id="14215-196">Klikněte na tlačítko **tabulky** položky v hello stromové zobrazení toosee obsahu z tabulky hello a pak klikněte na **upravit** tlačítko dole v hello.</span><span class="sxs-lookup"><span data-stu-id="14215-196">Click a **table** item in hello tree view toosee content from hello table and then click **Edit** button at hello bottom.</span></span>  

   ![Dialogové okno Navigátor](./media/data-factory-web-table-connector/Navigator-DialogBox.png)
6. <span data-ttu-id="14215-198">V hello **Editor dotazů** okně klikněte na tlačítko **Upřesnit** tlačítka na panelu nástrojů hello.</span><span class="sxs-lookup"><span data-stu-id="14215-198">In hello **Query Editor** window, click **Advanced Editor** button on hello toolbar.</span></span>

    ![Tlačítko Rozšířené editoru](./media/data-factory-web-table-connector/QueryEditor-AdvancedEditorButton.png)
7. <span data-ttu-id="14215-200">V dialogové okno Upřesnit hello, hello číslo vedle příliš "Zdroj" je hello index.</span><span class="sxs-lookup"><span data-stu-id="14215-200">In hello Advanced Editor dialog box, hello number next too"Source" is hello index.</span></span>

    ![Rozšířené Editor - indexu](./media/data-factory-web-table-connector/AdvancedEditor-Index.png)

<span data-ttu-id="14215-202">Pokud používáte Excel 2013, použijte [Microsoft Power Query pro Excel](https://www.microsoft.com/download/details.aspx?id=39379) tooget hello index.</span><span class="sxs-lookup"><span data-stu-id="14215-202">If you are using Excel 2013, use [Microsoft Power Query for Excel](https://www.microsoft.com/download/details.aspx?id=39379) tooget hello index.</span></span> <span data-ttu-id="14215-203">V tématu [Connect tooa webové stránky](https://support.office.com/article/Connect-to-a-web-page-Power-Query-b2725d67-c9e8-43e6-a590-c0a175bd64d8) článku.</span><span class="sxs-lookup"><span data-stu-id="14215-203">See [Connect tooa web page](https://support.office.com/article/Connect-to-a-web-page-Power-Query-b2725d67-c9e8-43e6-a590-c0a175bd64d8) article for details.</span></span> <span data-ttu-id="14215-204">Hello kroky jsou podobné, pokud používáte [Microsoft Power BI pro plochu](https://powerbi.microsoft.com/desktop/).</span><span class="sxs-lookup"><span data-stu-id="14215-204">hello steps are similar if you are using [Microsoft Power BI for Desktop](https://powerbi.microsoft.com/desktop/).</span></span>

> [!NOTE]
> <span data-ttu-id="14215-205">toomap sloupce z toocolumns datové sady zdroje z podřízený datové sady, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="14215-205">toomap columns from source dataset toocolumns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="14215-206">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="14215-206">Performance and Tuning</span></span>
<span data-ttu-id="14215-207">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.</span><span class="sxs-lookup"><span data-stu-id="14215-207">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
