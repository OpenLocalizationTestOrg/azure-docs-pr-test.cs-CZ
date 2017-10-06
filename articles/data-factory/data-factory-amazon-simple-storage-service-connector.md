---
title: "aaaMove data ze služby Amazon jednoduché úložiště pomocí služby Data Factory | Microsoft Docs"
description: "Další informace o tom toomove data ze služby úložiště jednoduché Amazon (S3) pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 636d3179-eba8-4841-bcb4-3563f6822a26
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: 8a8cd2845fd1de74413bd0372f3aabfb4817549b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-amazon-simple-storage-service-by-using-azure-data-factory"></a><span data-ttu-id="4771b-103">Přesun dat ze služby Amazon jednoduché úložiště pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="4771b-103">Move data from Amazon Simple Storage Service by using Azure Data Factory</span></span>
<span data-ttu-id="4771b-104">Tento článek vysvětluje, jak toouse hello aktivita kopírování v Azure Data Factory toomove data ze služby úložiště jednoduché Amazon (S3).</span><span class="sxs-lookup"><span data-stu-id="4771b-104">This article explains how toouse hello copy activity in Azure Data Factory toomove data from Amazon Simple Storage Service (S3).</span></span> <span data-ttu-id="4771b-105">Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="4771b-105">It builds on hello [Data movement activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="4771b-106">Může kopírovat data z úložiště dat podřízený Amazon S3 tooany podporována.</span><span class="sxs-lookup"><span data-stu-id="4771b-106">You can copy data from Amazon S3 tooany supported sink data store.</span></span> <span data-ttu-id="4771b-107">Seznam dat úložiště, které jsou podporované jako jímky pomocí aktivity kopírování hello, najdete v části hello [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky.</span><span class="sxs-lookup"><span data-stu-id="4771b-107">For a list of data stores supported as sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="4771b-108">Objekt pro vytváření dat aktuálně podporuje pouze přesunutí dat z úložiště dat tooother Amazon S3, ale není přesouvání dat od ostatních dat ukládá tooAmazon S3.</span><span class="sxs-lookup"><span data-stu-id="4771b-108">Data Factory currently supports only moving data from Amazon S3 tooother data stores, but not moving data from other data stores tooAmazon S3.</span></span>

## <a name="required-permissions"></a><span data-ttu-id="4771b-109">Požadovaná oprávnění</span><span class="sxs-lookup"><span data-stu-id="4771b-109">Required permissions</span></span>
<span data-ttu-id="4771b-110">toocopy data z Amazonu S3, zajistěte, aby vám byla udělena hello následující oprávnění:</span><span class="sxs-lookup"><span data-stu-id="4771b-110">toocopy data from Amazon S3, make sure you have been granted hello following permissions:</span></span>

* <span data-ttu-id="4771b-111">`s3:GetObject`a `s3:GetObjectVersion` pro Amazon S3 objekt operace.</span><span class="sxs-lookup"><span data-stu-id="4771b-111">`s3:GetObject` and `s3:GetObjectVersion` for Amazon S3 Object Operations.</span></span>
* <span data-ttu-id="4771b-112">`s3:ListBucket`pro operace sady Amazon S3.</span><span class="sxs-lookup"><span data-stu-id="4771b-112">`s3:ListBucket` for Amazon S3 Bucket Operations.</span></span> <span data-ttu-id="4771b-113">Pokud používáte hello Průvodce kopírováním objekt pro vytváření dat, `s3:ListAllMyBuckets` je také nutný.</span><span class="sxs-lookup"><span data-stu-id="4771b-113">If you are using hello Data Factory Copy Wizard, `s3:ListAllMyBuckets` is also required.</span></span>

<span data-ttu-id="4771b-114">Podrobnosti o hello úplný seznam Amazon S3 oprávnění najdete v tématu [zadání oprávnění v zásadách](http://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).</span><span class="sxs-lookup"><span data-stu-id="4771b-114">For details about hello full list of Amazon S3 permissions, see [Specifying Permissions in a Policy](http://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).</span></span>

## <a name="getting-started"></a><span data-ttu-id="4771b-115">Začínáme</span><span class="sxs-lookup"><span data-stu-id="4771b-115">Getting started</span></span>
<span data-ttu-id="4771b-116">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z zdroje Amazon S3 s použitím rozhraní API nebo jiných nástrojů.</span><span class="sxs-lookup"><span data-stu-id="4771b-116">You can create a pipeline with a copy activity that moves data from an Amazon S3 source by using different tools or APIs.</span></span>

<span data-ttu-id="4771b-117">Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="4771b-117">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="4771b-118">Rychlé návod najdete v tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="4771b-118">For a quick walkthrough, see [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md).</span></span>

<span data-ttu-id="4771b-119">Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**.</span><span class="sxs-lookup"><span data-stu-id="4771b-119">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="4771b-120">Podrobné pokyny toocreate kanál s aktivitou kopírování, najdete v části hello [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="4771b-120">For step-by-step instructions toocreate a pipeline with a copy activity, see hello [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>

<span data-ttu-id="4771b-121">Jestli používáte nástroje nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:</span><span class="sxs-lookup"><span data-stu-id="4771b-121">Whether you use tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="4771b-122">Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="4771b-122">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="4771b-123">Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="4771b-123">Create **datasets** toorepresent input and output data for hello copy operation.</span></span>
3. <span data-ttu-id="4771b-124">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="4771b-124">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span>

<span data-ttu-id="4771b-125">Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello).</span><span class="sxs-lookup"><span data-stu-id="4771b-125">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="4771b-126">Pokud používáte rozhraní API (s výjimkou .NET API) nebo nástroje, definujete tyto entity služby Data Factory pomocí formátu JSON hello.</span><span class="sxs-lookup"><span data-stu-id="4771b-126">When you use tools or APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span> <span data-ttu-id="4771b-127">Ukázka s definicemi JSON entit služby Data Factory, které jsou používané toocopy dat z úložiště dat Amazon S3, najdete v části hello [JSON příklad: kopírování dat z Amazon S3 tooAzure Blob](#json-example-copy-data-from-amazon-s3-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="4771b-127">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an Amazon S3 data store, see hello [JSON example: Copy data from Amazon S3 tooAzure Blob](#json-example-copy-data-from-amazon-s3-to-azure-blob) section of this article.</span></span>

> [!NOTE]
> <span data-ttu-id="4771b-128">Podrobnosti o podporovaných formátech souborů a komprese pro aktivitu kopírování najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md).</span><span class="sxs-lookup"><span data-stu-id="4771b-128">For details about supported file and compression formats for a copy activity, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md).</span></span>

<span data-ttu-id="4771b-129">Hello následující části obsahují podrobné informace o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooAmazon S3.</span><span class="sxs-lookup"><span data-stu-id="4771b-129">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooAmazon S3.</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="4771b-130">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="4771b-130">Linked service properties</span></span>
<span data-ttu-id="4771b-131">Propojená služba odkazuje data store tooa objekt pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="4771b-131">A linked service links a data store tooa data factory.</span></span> <span data-ttu-id="4771b-132">Vytvoření propojené služby typu **AwsAccessKey** toolink Amazon S3 data ukládat tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="4771b-132">You create a linked service of type **AwsAccessKey** toolink your Amazon S3 data store tooyour data factory.</span></span> <span data-ttu-id="4771b-133">Hello následující tabulka obsahuje popis JSON elementy konkrétní tooAmazon S3 (AwsAccessKey) propojené služby.</span><span class="sxs-lookup"><span data-stu-id="4771b-133">hello following table provides description for JSON elements specific tooAmazon S3 (AwsAccessKey) linked service.</span></span>

| <span data-ttu-id="4771b-134">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="4771b-134">Property</span></span> | <span data-ttu-id="4771b-135">Popis</span><span class="sxs-lookup"><span data-stu-id="4771b-135">Description</span></span> | <span data-ttu-id="4771b-136">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="4771b-136">Allowed values</span></span> | <span data-ttu-id="4771b-137">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="4771b-137">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="4771b-138">accessKeyID</span><span class="sxs-lookup"><span data-stu-id="4771b-138">accessKeyID</span></span> |<span data-ttu-id="4771b-139">ID hello tajný přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="4771b-139">ID of hello secret access key.</span></span> |<span data-ttu-id="4771b-140">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4771b-140">string</span></span> |<span data-ttu-id="4771b-141">Ano</span><span class="sxs-lookup"><span data-stu-id="4771b-141">Yes</span></span> |
| <span data-ttu-id="4771b-142">secretAccessKey</span><span class="sxs-lookup"><span data-stu-id="4771b-142">secretAccessKey</span></span> |<span data-ttu-id="4771b-143">Hello tajný přístupový klíč, sám sebe.</span><span class="sxs-lookup"><span data-stu-id="4771b-143">hello secret access key itself.</span></span> |<span data-ttu-id="4771b-144">Šifrované tajné řetězec</span><span class="sxs-lookup"><span data-stu-id="4771b-144">Encrypted secret string</span></span> |<span data-ttu-id="4771b-145">Ano</span><span class="sxs-lookup"><span data-stu-id="4771b-145">Yes</span></span> |

<span data-ttu-id="4771b-146">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="4771b-146">Here is an example:</span></span>

```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AwsAccessKey",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": "<secret access key>"
        }
    }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="4771b-147">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="4771b-147">Dataset properties</span></span>
<span data-ttu-id="4771b-148">toospecify toorepresent datovou sadu vstupní data v úložišti objektů Blob v Azure, nastavte vlastnost typu hello sady dat hello příliš**AmazonS3**.</span><span class="sxs-lookup"><span data-stu-id="4771b-148">toospecify a dataset toorepresent input data in Azure Blob storage, set hello type property of hello dataset too**AmazonS3**.</span></span> <span data-ttu-id="4771b-149">Sada hello **linkedServiceName** vlastnost hello datovou sadu toohello název hello Amazon S3 propojené služby.</span><span class="sxs-lookup"><span data-stu-id="4771b-149">Set hello **linkedServiceName** property of hello dataset toohello name of hello Amazon S3 linked service.</span></span> <span data-ttu-id="4771b-150">Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části [vytváření datových sad](data-factory-create-datasets.md).</span><span class="sxs-lookup"><span data-stu-id="4771b-150">For a full list of sections and properties available for defining datasets, see [Creating datasets](data-factory-create-datasets.md).</span></span> 

<span data-ttu-id="4771b-151">Oddíly jako je například struktura, dostupnost a zásady jsou podobné pro všechny typy datové sady (například databáze SQL, objektů blob v Azure a tabulky Azure).</span><span class="sxs-lookup"><span data-stu-id="4771b-151">Sections such as structure, availability, and policy are similar for all dataset types (such as SQL database, Azure blob, and Azure table).</span></span> <span data-ttu-id="4771b-152">Hello **rámci typeProperties** části se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello.</span><span class="sxs-lookup"><span data-stu-id="4771b-152">hello **typeProperties** section is different for each type of dataset, and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="4771b-153">Hello **rámci typeProperties** části datové sady typu **AmazonS3** (což zahrnuje datová sada hello Amazon S3) má hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="4771b-153">hello **typeProperties** section for a dataset of type **AmazonS3** (which includes hello Amazon S3 dataset) has hello following properties:</span></span>

| <span data-ttu-id="4771b-154">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="4771b-154">Property</span></span> | <span data-ttu-id="4771b-155">Popis</span><span class="sxs-lookup"><span data-stu-id="4771b-155">Description</span></span> | <span data-ttu-id="4771b-156">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="4771b-156">Allowed values</span></span> | <span data-ttu-id="4771b-157">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="4771b-157">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="4771b-158">bucketName</span><span class="sxs-lookup"><span data-stu-id="4771b-158">bucketName</span></span> |<span data-ttu-id="4771b-159">Název sady Hello S3.</span><span class="sxs-lookup"><span data-stu-id="4771b-159">hello S3 bucket name.</span></span> |<span data-ttu-id="4771b-160">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4771b-160">String</span></span> |<span data-ttu-id="4771b-161">Ano</span><span class="sxs-lookup"><span data-stu-id="4771b-161">Yes</span></span> |
| <span data-ttu-id="4771b-162">key</span><span class="sxs-lookup"><span data-stu-id="4771b-162">key</span></span> |<span data-ttu-id="4771b-163">klíč objektu Hello S3.</span><span class="sxs-lookup"><span data-stu-id="4771b-163">hello S3 object key.</span></span> |<span data-ttu-id="4771b-164">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4771b-164">String</span></span> |<span data-ttu-id="4771b-165">Ne</span><span class="sxs-lookup"><span data-stu-id="4771b-165">No</span></span> |
| <span data-ttu-id="4771b-166">Předpona</span><span class="sxs-lookup"><span data-stu-id="4771b-166">prefix</span></span> |<span data-ttu-id="4771b-167">Předpona pro klíč objektu S3 hello.</span><span class="sxs-lookup"><span data-stu-id="4771b-167">Prefix for hello S3 object key.</span></span> <span data-ttu-id="4771b-168">Jsou vybrané objekty, jejichž klíče začít s touto předponou.</span><span class="sxs-lookup"><span data-stu-id="4771b-168">Objects whose keys start with this prefix are selected.</span></span> <span data-ttu-id="4771b-169">Platí pouze v případě, klíč je prázdný.</span><span class="sxs-lookup"><span data-stu-id="4771b-169">Applies only when key is empty.</span></span> |<span data-ttu-id="4771b-170">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4771b-170">String</span></span> |<span data-ttu-id="4771b-171">Ne</span><span class="sxs-lookup"><span data-stu-id="4771b-171">No</span></span> |
| <span data-ttu-id="4771b-172">Verze</span><span class="sxs-lookup"><span data-stu-id="4771b-172">version</span></span> |<span data-ttu-id="4771b-173">verze Hello hello S3 objektu, pokud je povolena Správa verzí S3.</span><span class="sxs-lookup"><span data-stu-id="4771b-173">hello version of hello S3 object, if S3 versioning is enabled.</span></span> |<span data-ttu-id="4771b-174">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4771b-174">String</span></span> |<span data-ttu-id="4771b-175">Ne</span><span class="sxs-lookup"><span data-stu-id="4771b-175">No</span></span> |
| <span data-ttu-id="4771b-176">Formát</span><span class="sxs-lookup"><span data-stu-id="4771b-176">format</span></span> | <span data-ttu-id="4771b-177">jsou podporovány následující typy formátu Hello: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="4771b-177">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="4771b-178">Sada hello **typ** vlastnost pod formátu tooone z těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="4771b-178">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="4771b-179">Další informace najdete v tématu hello [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu JSON](data-factory-supported-file-and-compression-formats.md#json-format), [formát Avro](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formátu ](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly.</span><span class="sxs-lookup"><span data-stu-id="4771b-179">For more information, see hello [Text format](data-factory-supported-file-and-compression-formats.md#text-format), [JSON format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="4771b-180">Pokud chcete, aby soubory toocopy jako-mezi souborové úložiště (binární kopie), přeskočit hello formátu část v obou definice vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="4771b-180">If you want toocopy files as-is between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="4771b-181">Ne</span><span class="sxs-lookup"><span data-stu-id="4771b-181">No</span></span> | |
| <span data-ttu-id="4771b-182">Komprese</span><span class="sxs-lookup"><span data-stu-id="4771b-182">compression</span></span> | <span data-ttu-id="4771b-183">Zadejte typ hello a úroveň komprese dat hello.</span><span class="sxs-lookup"><span data-stu-id="4771b-183">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="4771b-184">Hello podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="4771b-184">hello supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="4771b-185">jsou Hello podporované úrovně: **Optimal** a **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="4771b-185">hello supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="4771b-186">Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="4771b-186">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="4771b-187">Ne</span><span class="sxs-lookup"><span data-stu-id="4771b-187">No</span></span> | |


> [!NOTE]
> <span data-ttu-id="4771b-188">**bucketName + klíč** Určuje umístění hello hello S3 objektu, kde je sady hello Kořenový kontejner pro objekty S3 a klíč je objekt toohello S3 hello úplnou cestu.</span><span class="sxs-lookup"><span data-stu-id="4771b-188">**bucketName + key** specifies hello location of hello S3 object, where bucket is hello root container for S3 objects, and key is hello full path toohello S3 object.</span></span>

### <a name="sample-dataset-with-prefix"></a><span data-ttu-id="4771b-189">Ukázkovou datovou sadu s předponou</span><span class="sxs-lookup"><span data-stu-id="4771b-189">Sample dataset with prefix</span></span>

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "prefix": "testFolder/test",
            "bucketName": "testbucket",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
### <a name="sample-dataset-with-version"></a><span data-ttu-id="4771b-190">Ukázkovou datovou sadu (verze)</span><span class="sxs-lookup"><span data-stu-id="4771b-190">Sample dataset (with version)</span></span>

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "key": "testFolder/test.orc",
            "bucketName": "testbucket",
            "version": "XXXXXXXXXczm0CJajYkHf0_k6LhBmkcL",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

### <a name="dynamic-paths-for-s3"></a><span data-ttu-id="4771b-191">Dynamické cesty pro S3</span><span class="sxs-lookup"><span data-stu-id="4771b-191">Dynamic paths for S3</span></span>
<span data-ttu-id="4771b-192">Hello předchozí ukázce se používá pevné hodnoty pro hello **klíč** a **bucketName** vlastnosti v datové sadě hello Amazon S3.</span><span class="sxs-lookup"><span data-stu-id="4771b-192">hello preceding sample uses fixed values for hello **key** and **bucketName** properties in hello Amazon S3 dataset.</span></span>

```json
"key": "testFolder/test.orc",
"bucketName": "testbucket",
```

<span data-ttu-id="4771b-193">Můžete mít vypočítat tyto vlastnosti dynamicky za běhu, pomocí systémové proměnné, jako je například SliceStart služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="4771b-193">You can have Data Factory calculate these properties dynamically at runtime, by using system variables such as SliceStart.</span></span>

```json
"key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
"bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"
```

<span data-ttu-id="4771b-194">Můžete provést stejný hello pro hello **předponu** vlastnost datové sadě služby Amazon S3.</span><span class="sxs-lookup"><span data-stu-id="4771b-194">You can do hello same for hello **prefix** property of an Amazon S3 dataset.</span></span> <span data-ttu-id="4771b-195">Seznam podporovaných funkce a proměnné, naleznete v části [funkce pro vytváření dat a systémové proměnné](data-factory-functions-variables.md).</span><span class="sxs-lookup"><span data-stu-id="4771b-195">For a list of supported functions and variables, see [Data Factory functions and system variables](data-factory-functions-variables.md).</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="4771b-196">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="4771b-196">Copy activity properties</span></span>
<span data-ttu-id="4771b-197">Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu [vytváření kanálů](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="4771b-197">For a full list of sections and properties available for defining activities, see [Creating pipelines](data-factory-create-pipelines.md).</span></span> <span data-ttu-id="4771b-198">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="4771b-198">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span> <span data-ttu-id="4771b-199">Vlastnosti, které jsou k dispozici v hello **rámci typeProperties** části hello aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="4771b-199">Properties available in hello **typeProperties** section of hello activity vary with each activity type.</span></span> <span data-ttu-id="4771b-200">Pro aktivitu kopírování hello vlastnosti lišit v závislosti na typech hello zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="4771b-200">For hello copy activity, properties vary depending on hello types of sources and sinks.</span></span> <span data-ttu-id="4771b-201">Pokud je zdroj v aktivitě kopírování hello typu **FileSystemSource** (která zahrnuje Amazon S3), hello následující vlastnosti jsou k dispozici v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="4771b-201">When a source in hello copy activity is of type **FileSystemSource** (which includes Amazon S3), hello following property is available in **typeProperties** section:</span></span>

| <span data-ttu-id="4771b-202">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="4771b-202">Property</span></span> | <span data-ttu-id="4771b-203">Popis</span><span class="sxs-lookup"><span data-stu-id="4771b-203">Description</span></span> | <span data-ttu-id="4771b-204">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="4771b-204">Allowed values</span></span> | <span data-ttu-id="4771b-205">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="4771b-205">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="4771b-206">Rekurzivní</span><span class="sxs-lookup"><span data-stu-id="4771b-206">recursive</span></span> |<span data-ttu-id="4771b-207">Určuje, zda seznam toorecursively S3 objekty v adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="4771b-207">Specifies whether toorecursively list S3 objects under hello directory.</span></span> |<span data-ttu-id="4771b-208">hodnotu true nebo false</span><span class="sxs-lookup"><span data-stu-id="4771b-208">true/false</span></span> |<span data-ttu-id="4771b-209">Ne</span><span class="sxs-lookup"><span data-stu-id="4771b-209">No</span></span> |

## <a name="json-example-copy-data-from-amazon-s3-tooazure-blob-storage"></a><span data-ttu-id="4771b-210">Příklad JSON: kopírování dat z Amazon S3 tooAzure úložiště objektů Blob</span><span class="sxs-lookup"><span data-stu-id="4771b-210">JSON example: Copy data from Amazon S3 tooAzure Blob storage</span></span>
<span data-ttu-id="4771b-211">Tento příklad ukazuje, jak toocopy data z Amazon S3 tooan úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="4771b-211">This sample shows how toocopy data from Amazon S3 tooan Azure Blob storage.</span></span> <span data-ttu-id="4771b-212">Ale data se dají zkopírovat přímo příliš[žádné hello jímky, které jsou podporovány](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí aktivity kopírování hello v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="4771b-212">However, data can be copied directly too[any of hello sinks that are supported](data-factory-data-movement-activities.md#supported-data-stores-and-formats) by using hello copy activity in Data Factory.</span></span>

<span data-ttu-id="4771b-213">Ukázka Hello poskytuje JSON definice hello následující entity služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="4771b-213">hello sample provides JSON definitions for hello following Data Factory entities.</span></span> <span data-ttu-id="4771b-214">Můžete použít tyto definice toocreate kanálu toocopy dat z úložiště tooBlob Amazon S3, pomocí hello [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), nebo [prostředí PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="4771b-214">You can use these definitions toocreate a pipeline toocopy data from Amazon S3 tooBlob storage, by using hello [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), or [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span>   

* <span data-ttu-id="4771b-215">Propojené služby typu [AwsAccessKey](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="4771b-215">A linked service of type [AwsAccessKey](#linked-service-properties).</span></span>
* <span data-ttu-id="4771b-216">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="4771b-216">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="4771b-217">Vstup [datovou sadu](data-factory-create-datasets.md) typu [AmazonS3](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="4771b-217">An input [dataset](data-factory-create-datasets.md) of type [AmazonS3](#dataset-properties).</span></span>
* <span data-ttu-id="4771b-218">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="4771b-218">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="4771b-219">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [FileSystemSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="4771b-219">A [pipeline](data-factory-create-pipelines.md) with copy activity that uses [FileSystemSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="4771b-220">Ukázka Hello zkopíruje data z Amazon S3 tooan objektů blob v Azure každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="4771b-220">hello sample copies data from Amazon S3 tooan Azure blob every hour.</span></span> <span data-ttu-id="4771b-221">Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.</span><span class="sxs-lookup"><span data-stu-id="4771b-221">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

### <a name="amazon-s3-linked-service"></a><span data-ttu-id="4771b-222">Amazon S3 propojené služby</span><span class="sxs-lookup"><span data-stu-id="4771b-222">Amazon S3 linked service</span></span>

```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AwsAccessKey",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": "<secret access key>"
        }
    }
}
```

### <a name="azure-storage-linked-service"></a><span data-ttu-id="4771b-223">Propojená služba Azure Storage</span><span class="sxs-lookup"><span data-stu-id="4771b-223">Azure Storage linked service</span></span>

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

### <a name="amazon-s3-input-dataset"></a><span data-ttu-id="4771b-224">Vstupní datové sady Amazon S3</span><span class="sxs-lookup"><span data-stu-id="4771b-224">Amazon S3 input dataset</span></span>

<span data-ttu-id="4771b-225">Nastavení **"externí": true** informuje o tom služba Data Factory hello tuto datovou sadu hello je externí toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="4771b-225">Setting **"external": true** informs hello Data Factory service that hello dataset is external toohello data factory.</span></span> <span data-ttu-id="4771b-226">Nastavte tuto vlastnost tootrue vstupní datovou sadu, která není vyprodukované aktivitu v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="4771b-226">Set this property tootrue on an input dataset that is not produced by an activity in hello pipeline.</span></span>

```json
    {
        "name": "AmazonS3InputDataset",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "AmazonS3LinkedService",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }
```


### <a name="azure-blob-output-dataset"></a><span data-ttu-id="4771b-227">Výstupní datová sada Azure Blob</span><span class="sxs-lookup"><span data-stu-id="4771b-227">Azure Blob output dataset</span></span>

<span data-ttu-id="4771b-228">Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="4771b-228">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="4771b-229">Cesta ke složce Hello pro objekt blob hello je vyhodnocován dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="4771b-229">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="4771b-230">Cesta ke složce Hello používá hello rok, měsíc, den a čas části hello počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="4771b-230">hello folder path uses hello year, month, day, and hours parts of hello start time.</span></span>

```json
{
    "name": "AzureBlobOutputDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/fromamazons3/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
            "partitionedBy": [
                {
                    "name": "Year",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "yyyy"
                    }
                },
                {
                    "name": "Month",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "MM"
                    }
                },
                {
                    "name": "Day",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "dd"
                    }
                },
                {
                    "name": "Hour",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "HH"
                    }
                }
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```


### <a name="copy-activity-in-a-pipeline-with-an-amazon-s3-source-and-a-blob-sink"></a><span data-ttu-id="4771b-231">Aktivita kopírování v kanálu s na Amazon S3 zdroj a jímka objektů blob</span><span class="sxs-lookup"><span data-stu-id="4771b-231">Copy activity in a pipeline with an Amazon S3 source and a blob sink</span></span>

<span data-ttu-id="4771b-232">Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady, a je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="4771b-232">hello pipeline contains a copy activity that is configured toouse hello input and output datasets, and is scheduled toorun every hour.</span></span> <span data-ttu-id="4771b-233">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**FileSystemSource**, a **podřízený** je typ nastaven příliš**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="4771b-233">In hello pipeline JSON definition, hello **source** type is set too**FileSystemSource**, and **sink** type is set too**BlobSink**.</span></span>

```json
{
    "name": "CopyAmazonS3ToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "FileSystemSource",
                        "recursive": true
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "AmazonS3InputDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutputDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "AmazonS3ToBlob"
            }
        ],
        "start": "2014-08-08T18:00:00Z",
        "end": "2014-08-08T19:00:00Z"
    }
}
```
> [!NOTE]
> <span data-ttu-id="4771b-234">toomap sloupce z toocolumns datovou sadu zdroj z podřízený datové sady, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="4771b-234">toomap columns from a source dataset toocolumns from a sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="4771b-235">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4771b-235">Next steps</span></span>
<span data-ttu-id="4771b-236">V tématu hello následující články:</span><span class="sxs-lookup"><span data-stu-id="4771b-236">See hello following articles:</span></span>

* <span data-ttu-id="4771b-237">toolearn o klíči faktory, že dopad výkonu (aktivita kopírování) přesun dat v datové továrně a různé způsoby toooptimize, najdete v části hello [zkopírujte aktivity výkonu a vyladění průvodce](data-factory-copy-activity-performance.md).</span><span class="sxs-lookup"><span data-stu-id="4771b-237">toolearn about key factors that impact performance of data movement (copy activity) in Data Factory, and various ways toooptimize it, see hello [Copy activity performance and tuning guide](data-factory-copy-activity-performance.md).</span></span>

* <span data-ttu-id="4771b-238">Podrobné pokyny pro vytvoření kanálu s aktivitou kopírování najdete v tématu hello [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="4771b-238">For step-by-step instructions for creating a pipeline with a copy activity, see hello [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
