---
title: "Přesun dat ze služby Amazon jednoduché úložiště pomocí služby Data Factory | Microsoft Docs"
description: "Další informace o tom, jak přesunout data ze služby úložiště jednoduché Amazon (S3) pomocí Azure Data Factory."
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
ms.openlocfilehash: 3e21f7dfccc3b235071344a28c7d94f65e6bf9ac
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="move-data-from-amazon-simple-storage-service-by-using-azure-data-factory"></a><span data-ttu-id="3baeb-103">Přesun dat ze služby Amazon jednoduché úložiště pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="3baeb-103">Move data from Amazon Simple Storage Service by using Azure Data Factory</span></span>
<span data-ttu-id="3baeb-104">Tento článek vysvětluje, jak pomocí aktivity kopírování v Azure Data Factory pro přesun dat ze služby úložiště jednoduché Amazon (S3).</span><span class="sxs-lookup"><span data-stu-id="3baeb-104">This article explains how to use the copy activity in Azure Data Factory to move data from Amazon Simple Storage Service (S3).</span></span> <span data-ttu-id="3baeb-105">Vychází [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="3baeb-105">It builds on the [Data movement activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="3baeb-106">Do úložiště dat žádné podporované podřízený může kopírovat data z Amazonu S3.</span><span class="sxs-lookup"><span data-stu-id="3baeb-106">You can copy data from Amazon S3 to any supported sink data store.</span></span> <span data-ttu-id="3baeb-107">Seznam úložišť dat jako jímky nepodporuje aktivitě kopírování najdete v tématu [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky.</span><span class="sxs-lookup"><span data-stu-id="3baeb-107">For a list of data stores supported as sinks by the copy activity, see the [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="3baeb-108">Objekt pro vytváření dat aktuálně podporuje pouze přesunutí dat z Amazon S3 jiným úložištím dat, ale není přesouvání dat od ostatních dat ukládá do Amazon S3.</span><span class="sxs-lookup"><span data-stu-id="3baeb-108">Data Factory currently supports only moving data from Amazon S3 to other data stores, but not moving data from other data stores to Amazon S3.</span></span>

## <a name="required-permissions"></a><span data-ttu-id="3baeb-109">Požadovaná oprávnění</span><span class="sxs-lookup"><span data-stu-id="3baeb-109">Required permissions</span></span>
<span data-ttu-id="3baeb-110">Pokud chcete zkopírovat data z Amazonu S3, zkontrolujte, zda že máte následující oprávnění:</span><span class="sxs-lookup"><span data-stu-id="3baeb-110">To copy data from Amazon S3, make sure you have been granted the following permissions:</span></span>

* <span data-ttu-id="3baeb-111">`s3:GetObject`a `s3:GetObjectVersion` pro Amazon S3 objekt operace.</span><span class="sxs-lookup"><span data-stu-id="3baeb-111">`s3:GetObject` and `s3:GetObjectVersion` for Amazon S3 Object Operations.</span></span>
* <span data-ttu-id="3baeb-112">`s3:ListBucket`pro operace sady Amazon S3.</span><span class="sxs-lookup"><span data-stu-id="3baeb-112">`s3:ListBucket` for Amazon S3 Bucket Operations.</span></span> <span data-ttu-id="3baeb-113">Pokud použijete Průvodce kopírováním objekt pro vytváření dat `s3:ListAllMyBuckets` je také nutný.</span><span class="sxs-lookup"><span data-stu-id="3baeb-113">If you are using the Data Factory Copy Wizard, `s3:ListAllMyBuckets` is also required.</span></span>

<span data-ttu-id="3baeb-114">Podrobnosti o úplný seznam Amazon S3 oprávnění najdete v tématu [zadání oprávnění v zásadách](http://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).</span><span class="sxs-lookup"><span data-stu-id="3baeb-114">For details about the full list of Amazon S3 permissions, see [Specifying Permissions in a Policy](http://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).</span></span>

## <a name="getting-started"></a><span data-ttu-id="3baeb-115">Začínáme</span><span class="sxs-lookup"><span data-stu-id="3baeb-115">Getting started</span></span>
<span data-ttu-id="3baeb-116">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z zdroje Amazon S3 s použitím rozhraní API nebo jiných nástrojů.</span><span class="sxs-lookup"><span data-stu-id="3baeb-116">You can create a pipeline with a copy activity that moves data from an Amazon S3 source by using different tools or APIs.</span></span>

<span data-ttu-id="3baeb-117">Nejjednodušší způsob, jak vytvořit kanál je použití **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="3baeb-117">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="3baeb-118">Rychlé návod najdete v tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="3baeb-118">For a quick walkthrough, see [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md).</span></span>

<span data-ttu-id="3baeb-119">Tyto nástroje můžete také použít k vytvoření kanálu: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru**, **.NET API**, a **REST API**.</span><span class="sxs-lookup"><span data-stu-id="3baeb-119">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="3baeb-120">Podrobné pokyny k vytvoření kanálu s aktivitou kopírování najdete v tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="3baeb-120">For step-by-step instructions to create a pipeline with a copy activity, see the [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>

<span data-ttu-id="3baeb-121">Jestli používáte nástroje nebo rozhraní API, je třeba provést následující kroky k vytvoření kanálu, který přesouvá data ze zdrojového úložiště dat do úložiště dat podřízený:</span><span class="sxs-lookup"><span data-stu-id="3baeb-121">Whether you use tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="3baeb-122">Vytvoření **propojené služby** propojení vstupní a výstupní data ukládá do data factory.</span><span class="sxs-lookup"><span data-stu-id="3baeb-122">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="3baeb-123">Vytvoření **datové sady** představují vstupní a výstupní data pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="3baeb-123">Create **datasets** to represent input and output data for the copy operation.</span></span>
3. <span data-ttu-id="3baeb-124">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="3baeb-124">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span>

<span data-ttu-id="3baeb-125">Když použijete průvodce, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál).</span><span class="sxs-lookup"><span data-stu-id="3baeb-125">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="3baeb-126">Pokud používáte rozhraní API (s výjimkou .NET API) nebo nástroje, definujete tyto entity služby Data Factory pomocí formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="3baeb-126">When you use tools or APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span> <span data-ttu-id="3baeb-127">Ukázku s definicemi JSON entit služby Data Factory, které se používají ke zkopírování dat z úložiště dat Amazon S3, najdete [JSON příklad: kopírování dat z Amazon S3 do objektu Blob Azure](#json-example-copy-data-from-amazon-s3-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="3baeb-127">For a sample with JSON definitions for Data Factory entities that are used to copy data from an Amazon S3 data store, see the [JSON example: Copy data from Amazon S3 to Azure Blob](#json-example-copy-data-from-amazon-s3-to-azure-blob) section of this article.</span></span>

> [!NOTE]
> <span data-ttu-id="3baeb-128">Podrobnosti o podporovaných formátech souborů a komprese pro aktivitu kopírování najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md).</span><span class="sxs-lookup"><span data-stu-id="3baeb-128">For details about supported file and compression formats for a copy activity, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md).</span></span>

<span data-ttu-id="3baeb-129">Následující části obsahují podrobnosti o vlastnostech formátu JSON, které slouží k definování entit služby Data Factory, které jsou specifické pro Amazon S3.</span><span class="sxs-lookup"><span data-stu-id="3baeb-129">The following sections provide details about JSON properties that are used to define Data Factory entities specific to Amazon S3.</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="3baeb-130">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="3baeb-130">Linked service properties</span></span>
<span data-ttu-id="3baeb-131">Propojená služba odkazuje na objekt pro vytváření dat úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="3baeb-131">A linked service links a data store to a data factory.</span></span> <span data-ttu-id="3baeb-132">Vytvoření propojené služby typu **AwsAccessKey** propojit data store Amazon S3 s datovou továrnu.</span><span class="sxs-lookup"><span data-stu-id="3baeb-132">You create a linked service of type **AwsAccessKey** to link your Amazon S3 data store to your data factory.</span></span> <span data-ttu-id="3baeb-133">Následující tabulka obsahuje popis JSON elementy, které jsou specifické pro službu Amazon S3 (AwsAccessKey) propojená služba.</span><span class="sxs-lookup"><span data-stu-id="3baeb-133">The following table provides description for JSON elements specific to Amazon S3 (AwsAccessKey) linked service.</span></span>

| <span data-ttu-id="3baeb-134">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3baeb-134">Property</span></span> | <span data-ttu-id="3baeb-135">Popis</span><span class="sxs-lookup"><span data-stu-id="3baeb-135">Description</span></span> | <span data-ttu-id="3baeb-136">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3baeb-136">Allowed values</span></span> | <span data-ttu-id="3baeb-137">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3baeb-137">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3baeb-138">accessKeyID</span><span class="sxs-lookup"><span data-stu-id="3baeb-138">accessKeyID</span></span> |<span data-ttu-id="3baeb-139">ID tajný přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="3baeb-139">ID of the secret access key.</span></span> |<span data-ttu-id="3baeb-140">Řetězec</span><span class="sxs-lookup"><span data-stu-id="3baeb-140">string</span></span> |<span data-ttu-id="3baeb-141">Ano</span><span class="sxs-lookup"><span data-stu-id="3baeb-141">Yes</span></span> |
| <span data-ttu-id="3baeb-142">secretAccessKey</span><span class="sxs-lookup"><span data-stu-id="3baeb-142">secretAccessKey</span></span> |<span data-ttu-id="3baeb-143">Tajný přístupový klíč sám sebe.</span><span class="sxs-lookup"><span data-stu-id="3baeb-143">The secret access key itself.</span></span> |<span data-ttu-id="3baeb-144">Šifrované tajné řetězec</span><span class="sxs-lookup"><span data-stu-id="3baeb-144">Encrypted secret string</span></span> |<span data-ttu-id="3baeb-145">Ano</span><span class="sxs-lookup"><span data-stu-id="3baeb-145">Yes</span></span> |

<span data-ttu-id="3baeb-146">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="3baeb-146">Here is an example:</span></span>

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

## <a name="dataset-properties"></a><span data-ttu-id="3baeb-147">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="3baeb-147">Dataset properties</span></span>
<span data-ttu-id="3baeb-148">Chcete-li zadat datové sady představují vstupní data v úložišti objektů Blob v Azure, nastavte vlastnost typu datové sady, která **AmazonS3**.</span><span class="sxs-lookup"><span data-stu-id="3baeb-148">To specify a dataset to represent input data in Azure Blob storage, set the type property of the dataset to **AmazonS3**.</span></span> <span data-ttu-id="3baeb-149">Nastavte **linkedServiceName** vlastnosti datové sady, která název Amazon S3 propojené služby.</span><span class="sxs-lookup"><span data-stu-id="3baeb-149">Set the **linkedServiceName** property of the dataset to the name of the Amazon S3 linked service.</span></span> <span data-ttu-id="3baeb-150">Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části [vytváření datových sad](data-factory-create-datasets.md).</span><span class="sxs-lookup"><span data-stu-id="3baeb-150">For a full list of sections and properties available for defining datasets, see [Creating datasets](data-factory-create-datasets.md).</span></span> 

<span data-ttu-id="3baeb-151">Oddíly jako je například struktura, dostupnost a zásady jsou podobné pro všechny typy datové sady (například databáze SQL, objektů blob v Azure a tabulky Azure).</span><span class="sxs-lookup"><span data-stu-id="3baeb-151">Sections such as structure, availability, and policy are similar for all dataset types (such as SQL database, Azure blob, and Azure table).</span></span> <span data-ttu-id="3baeb-152">**Rámci typeProperties** části se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění dat v úložišti.</span><span class="sxs-lookup"><span data-stu-id="3baeb-152">The **typeProperties** section is different for each type of dataset, and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="3baeb-153">**Rámci typeProperties** části datové sady typu **AmazonS3** (což zahrnuje datová sada Amazon S3) má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="3baeb-153">The **typeProperties** section for a dataset of type **AmazonS3** (which includes the Amazon S3 dataset) has the following properties:</span></span>

| <span data-ttu-id="3baeb-154">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3baeb-154">Property</span></span> | <span data-ttu-id="3baeb-155">Popis</span><span class="sxs-lookup"><span data-stu-id="3baeb-155">Description</span></span> | <span data-ttu-id="3baeb-156">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3baeb-156">Allowed values</span></span> | <span data-ttu-id="3baeb-157">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3baeb-157">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3baeb-158">bucketName</span><span class="sxs-lookup"><span data-stu-id="3baeb-158">bucketName</span></span> |<span data-ttu-id="3baeb-159">Název sady S3.</span><span class="sxs-lookup"><span data-stu-id="3baeb-159">The S3 bucket name.</span></span> |<span data-ttu-id="3baeb-160">Řetězec</span><span class="sxs-lookup"><span data-stu-id="3baeb-160">String</span></span> |<span data-ttu-id="3baeb-161">Ano</span><span class="sxs-lookup"><span data-stu-id="3baeb-161">Yes</span></span> |
| <span data-ttu-id="3baeb-162">key</span><span class="sxs-lookup"><span data-stu-id="3baeb-162">key</span></span> |<span data-ttu-id="3baeb-163">Klíč objektu S3.</span><span class="sxs-lookup"><span data-stu-id="3baeb-163">The S3 object key.</span></span> |<span data-ttu-id="3baeb-164">Řetězec</span><span class="sxs-lookup"><span data-stu-id="3baeb-164">String</span></span> |<span data-ttu-id="3baeb-165">Ne</span><span class="sxs-lookup"><span data-stu-id="3baeb-165">No</span></span> |
| <span data-ttu-id="3baeb-166">Předpona</span><span class="sxs-lookup"><span data-stu-id="3baeb-166">prefix</span></span> |<span data-ttu-id="3baeb-167">Předpona pro klíč objektu S3.</span><span class="sxs-lookup"><span data-stu-id="3baeb-167">Prefix for the S3 object key.</span></span> <span data-ttu-id="3baeb-168">Jsou vybrané objekty, jejichž klíče začít s touto předponou.</span><span class="sxs-lookup"><span data-stu-id="3baeb-168">Objects whose keys start with this prefix are selected.</span></span> <span data-ttu-id="3baeb-169">Platí pouze v případě, klíč je prázdný.</span><span class="sxs-lookup"><span data-stu-id="3baeb-169">Applies only when key is empty.</span></span> |<span data-ttu-id="3baeb-170">Řetězec</span><span class="sxs-lookup"><span data-stu-id="3baeb-170">String</span></span> |<span data-ttu-id="3baeb-171">Ne</span><span class="sxs-lookup"><span data-stu-id="3baeb-171">No</span></span> |
| <span data-ttu-id="3baeb-172">Verze</span><span class="sxs-lookup"><span data-stu-id="3baeb-172">version</span></span> |<span data-ttu-id="3baeb-173">Verze objektu S3, pokud je povolena Správa verzí S3.</span><span class="sxs-lookup"><span data-stu-id="3baeb-173">The version of the S3 object, if S3 versioning is enabled.</span></span> |<span data-ttu-id="3baeb-174">Řetězec</span><span class="sxs-lookup"><span data-stu-id="3baeb-174">String</span></span> |<span data-ttu-id="3baeb-175">Ne</span><span class="sxs-lookup"><span data-stu-id="3baeb-175">No</span></span> |
| <span data-ttu-id="3baeb-176">Formát</span><span class="sxs-lookup"><span data-stu-id="3baeb-176">format</span></span> | <span data-ttu-id="3baeb-177">Jsou podporovány následující typy formátu: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="3baeb-177">The following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="3baeb-178">Nastavte **typ** vlastnost pod formát na jednu z těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="3baeb-178">Set the **type** property under format to one of these values.</span></span> <span data-ttu-id="3baeb-179">Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu JSON](data-factory-supported-file-and-compression-formats.md#json-format), [formát Avro](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly.</span><span class="sxs-lookup"><span data-stu-id="3baeb-179">For more information, see the [Text format](data-factory-supported-file-and-compression-formats.md#text-format), [JSON format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="3baeb-180">Pokud chcete zkopírovat soubory jako-je mezi souborové úložiště (binární kopie), přeskočte část formátu v obou definice vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="3baeb-180">If you want to copy files as-is between file-based stores (binary copy), skip the format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="3baeb-181">Ne</span><span class="sxs-lookup"><span data-stu-id="3baeb-181">No</span></span> | |
| <span data-ttu-id="3baeb-182">Komprese</span><span class="sxs-lookup"><span data-stu-id="3baeb-182">compression</span></span> | <span data-ttu-id="3baeb-183">Zadejte typ a úroveň komprese pro data.</span><span class="sxs-lookup"><span data-stu-id="3baeb-183">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="3baeb-184">Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="3baeb-184">The supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="3baeb-185">Jsou podporované úrovně: **Optimal** a **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="3baeb-185">The supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="3baeb-186">Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="3baeb-186">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="3baeb-187">Ne</span><span class="sxs-lookup"><span data-stu-id="3baeb-187">No</span></span> | |


> [!NOTE]
> <span data-ttu-id="3baeb-188">**bucketName + klíč** Určuje umístění objektu S3, kde sady je kořenový kontejner pro objekty S3 a klíč je úplná cesta k objektu S3.</span><span class="sxs-lookup"><span data-stu-id="3baeb-188">**bucketName + key** specifies the location of the S3 object, where bucket is the root container for S3 objects, and key is the full path to the S3 object.</span></span>

### <a name="sample-dataset-with-prefix"></a><span data-ttu-id="3baeb-189">Ukázkovou datovou sadu s předponou</span><span class="sxs-lookup"><span data-stu-id="3baeb-189">Sample dataset with prefix</span></span>

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
### <a name="sample-dataset-with-version"></a><span data-ttu-id="3baeb-190">Ukázkovou datovou sadu (verze)</span><span class="sxs-lookup"><span data-stu-id="3baeb-190">Sample dataset (with version)</span></span>

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

### <a name="dynamic-paths-for-s3"></a><span data-ttu-id="3baeb-191">Dynamické cesty pro S3</span><span class="sxs-lookup"><span data-stu-id="3baeb-191">Dynamic paths for S3</span></span>
<span data-ttu-id="3baeb-192">V předchozím příkladu používá pevné hodnoty **klíč** a **bucketName** vlastnosti v datové sadě Amazon S3.</span><span class="sxs-lookup"><span data-stu-id="3baeb-192">The preceding sample uses fixed values for the **key** and **bucketName** properties in the Amazon S3 dataset.</span></span>

```json
"key": "testFolder/test.orc",
"bucketName": "testbucket",
```

<span data-ttu-id="3baeb-193">Můžete mít vypočítat tyto vlastnosti dynamicky za běhu, pomocí systémové proměnné, jako je například SliceStart služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="3baeb-193">You can have Data Factory calculate these properties dynamically at runtime, by using system variables such as SliceStart.</span></span>

```json
"key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
"bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"
```

<span data-ttu-id="3baeb-194">Totéž proveďte pro **předponu** vlastnost datové sadě služby Amazon S3.</span><span class="sxs-lookup"><span data-stu-id="3baeb-194">You can do the same for the **prefix** property of an Amazon S3 dataset.</span></span> <span data-ttu-id="3baeb-195">Seznam podporovaných funkce a proměnné, naleznete v části [funkce pro vytváření dat a systémové proměnné](data-factory-functions-variables.md).</span><span class="sxs-lookup"><span data-stu-id="3baeb-195">For a list of supported functions and variables, see [Data Factory functions and system variables](data-factory-functions-variables.md).</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="3baeb-196">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="3baeb-196">Copy activity properties</span></span>
<span data-ttu-id="3baeb-197">Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu [vytváření kanálů](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="3baeb-197">For a full list of sections and properties available for defining activities, see [Creating pipelines](data-factory-create-pipelines.md).</span></span> <span data-ttu-id="3baeb-198">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="3baeb-198">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span> <span data-ttu-id="3baeb-199">Vlastnosti, které jsou k dispozici v **rámci typeProperties** části aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="3baeb-199">Properties available in the **typeProperties** section of the activity vary with each activity type.</span></span> <span data-ttu-id="3baeb-200">Pro aktivitu kopírování vlastnosti lišit v závislosti na typech zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="3baeb-200">For the copy activity, properties vary depending on the types of sources and sinks.</span></span> <span data-ttu-id="3baeb-201">Pokud je zdroj v aktivitě kopírování typu **FileSystemSource** (která zahrnuje Amazon S3), je k dispozici v této vlastnosti **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3baeb-201">When a source in the copy activity is of type **FileSystemSource** (which includes Amazon S3), the following property is available in **typeProperties** section:</span></span>

| <span data-ttu-id="3baeb-202">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3baeb-202">Property</span></span> | <span data-ttu-id="3baeb-203">Popis</span><span class="sxs-lookup"><span data-stu-id="3baeb-203">Description</span></span> | <span data-ttu-id="3baeb-204">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3baeb-204">Allowed values</span></span> | <span data-ttu-id="3baeb-205">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3baeb-205">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3baeb-206">Rekurzivní</span><span class="sxs-lookup"><span data-stu-id="3baeb-206">recursive</span></span> |<span data-ttu-id="3baeb-207">Určuje, jestli k rekurzivnímu seznamu S3 objekty v adresáři.</span><span class="sxs-lookup"><span data-stu-id="3baeb-207">Specifies whether to recursively list S3 objects under the directory.</span></span> |<span data-ttu-id="3baeb-208">hodnotu true nebo false</span><span class="sxs-lookup"><span data-stu-id="3baeb-208">true/false</span></span> |<span data-ttu-id="3baeb-209">Ne</span><span class="sxs-lookup"><span data-stu-id="3baeb-209">No</span></span> |

## <a name="json-example-copy-data-from-amazon-s3-to-azure-blob-storage"></a><span data-ttu-id="3baeb-210">Příklad JSON: kopírování dat z Amazonu S3 do úložiště objektů Blob v Azure</span><span class="sxs-lookup"><span data-stu-id="3baeb-210">JSON example: Copy data from Amazon S3 to Azure Blob storage</span></span>
<span data-ttu-id="3baeb-211">Tento příklad ukazuje, jak zkopírovat data z Amazonu S3 do Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="3baeb-211">This sample shows how to copy data from Amazon S3 to an Azure Blob storage.</span></span> <span data-ttu-id="3baeb-212">Ale data se dají zkopírovat přímo na [žádné jímky, které jsou podporovány](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí aktivity kopírování v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="3baeb-212">However, data can be copied directly to [any of the sinks that are supported](data-factory-data-movement-activities.md#supported-data-stores-and-formats) by using the copy activity in Data Factory.</span></span>

<span data-ttu-id="3baeb-213">Ukázka poskytuje JSON definice pro následující entity služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="3baeb-213">The sample provides JSON definitions for the following Data Factory entities.</span></span> <span data-ttu-id="3baeb-214">Tyto definice můžete použít k vytvoření kanálu zkopírovat data z Amazonu S3 do úložiště objektů Blob, pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), nebo [prostředí PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="3baeb-214">You can use these definitions to create a pipeline to copy data from Amazon S3 to Blob storage, by using the [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), or [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span>   

* <span data-ttu-id="3baeb-215">Propojené služby typu [AwsAccessKey](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="3baeb-215">A linked service of type [AwsAccessKey](#linked-service-properties).</span></span>
* <span data-ttu-id="3baeb-216">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="3baeb-216">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="3baeb-217">Vstup [datovou sadu](data-factory-create-datasets.md) typu [AmazonS3](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="3baeb-217">An input [dataset](data-factory-create-datasets.md) of type [AmazonS3](#dataset-properties).</span></span>
* <span data-ttu-id="3baeb-218">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="3baeb-218">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="3baeb-219">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [FileSystemSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="3baeb-219">A [pipeline](data-factory-create-pipelines.md) with copy activity that uses [FileSystemSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="3baeb-220">Ukázka zkopíruje data z Amazonu S3 do objektu blob Azure každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="3baeb-220">The sample copies data from Amazon S3 to an Azure blob every hour.</span></span> <span data-ttu-id="3baeb-221">Vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="3baeb-221">The JSON properties used in these samples are described in sections following the samples.</span></span>

### <a name="amazon-s3-linked-service"></a><span data-ttu-id="3baeb-222">Amazon S3 propojené služby</span><span class="sxs-lookup"><span data-stu-id="3baeb-222">Amazon S3 linked service</span></span>

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

### <a name="azure-storage-linked-service"></a><span data-ttu-id="3baeb-223">Propojená služba Azure Storage</span><span class="sxs-lookup"><span data-stu-id="3baeb-223">Azure Storage linked service</span></span>

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

### <a name="amazon-s3-input-dataset"></a><span data-ttu-id="3baeb-224">Vstupní datové sady Amazon S3</span><span class="sxs-lookup"><span data-stu-id="3baeb-224">Amazon S3 input dataset</span></span>

<span data-ttu-id="3baeb-225">Nastavení **"externí": true** informuje služba Data Factory, že je externí k objektu pro vytváření dat datové sady.</span><span class="sxs-lookup"><span data-stu-id="3baeb-225">Setting **"external": true** informs the Data Factory service that the dataset is external to the data factory.</span></span> <span data-ttu-id="3baeb-226">Nastavte tuto vlastnost na hodnotu true vstupní datovou sadu, která není vyprodukované aktivitu v kanálu.</span><span class="sxs-lookup"><span data-stu-id="3baeb-226">Set this property to true on an input dataset that is not produced by an activity in the pipeline.</span></span>

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


### <a name="azure-blob-output-dataset"></a><span data-ttu-id="3baeb-227">Výstupní datová sada Azure Blob</span><span class="sxs-lookup"><span data-stu-id="3baeb-227">Azure Blob output dataset</span></span>

<span data-ttu-id="3baeb-228">Data se zapisují do nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="3baeb-228">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="3baeb-229">Cesta ke složce pro tento objekt blob je vyhodnocován dynamicky podle času zahájení řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="3baeb-229">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="3baeb-230">Cesta ke složce používá rok, měsíc, den a čas části čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="3baeb-230">The folder path uses the year, month, day, and hours parts of the start time.</span></span>

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


### <a name="copy-activity-in-a-pipeline-with-an-amazon-s3-source-and-a-blob-sink"></a><span data-ttu-id="3baeb-231">Aktivita kopírování v kanálu s na Amazon S3 zdroj a jímka objektů blob</span><span class="sxs-lookup"><span data-stu-id="3baeb-231">Copy activity in a pipeline with an Amazon S3 source and a blob sink</span></span>

<span data-ttu-id="3baeb-232">Kanál obsahuje aktivitu kopírování, který je nakonfigurovaný na použití vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="3baeb-232">The pipeline contains a copy activity that is configured to use the input and output datasets, and is scheduled to run every hour.</span></span> <span data-ttu-id="3baeb-233">V definici JSON kanálu **zdroj** je typ nastaven na **FileSystemSource**, a **podřízený** je typ nastaven na **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="3baeb-233">In the pipeline JSON definition, the **source** type is set to **FileSystemSource**, and **sink** type is set to **BlobSink**.</span></span>

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
> <span data-ttu-id="3baeb-234">Mapování sloupců z datové sady zdroje na sloupce ze sady dat podřízený naleznete v tématu [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="3baeb-234">To map columns from a source dataset to columns from a sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="3baeb-235">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3baeb-235">Next steps</span></span>
<span data-ttu-id="3baeb-236">Viz následující články:</span><span class="sxs-lookup"><span data-stu-id="3baeb-236">See the following articles:</span></span>

* <span data-ttu-id="3baeb-237">Další informace o klíčových faktorů této dopad výkon přesun dat (aktivita kopírování) v objektu pro vytváření dat a různé způsoby, jak ji optimalizovat, najdete v článku [zkopírujte aktivity výkonu a vyladění průvodce](data-factory-copy-activity-performance.md).</span><span class="sxs-lookup"><span data-stu-id="3baeb-237">To learn about key factors that impact performance of data movement (copy activity) in Data Factory, and various ways to optimize it, see the [Copy activity performance and tuning guide](data-factory-copy-activity-performance.md).</span></span>

* <span data-ttu-id="3baeb-238">Podrobné pokyny pro vytvoření kanálu s aktivitou kopírování najdete v tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="3baeb-238">For step-by-step instructions for creating a pipeline with a copy activity, see the [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
