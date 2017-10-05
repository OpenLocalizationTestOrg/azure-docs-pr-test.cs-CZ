---
title: "Kopírování dat do/z Azure Blob Storage | Microsoft Docs"
description: "Zjistěte, jak zkopírovat data objektů blob v Azure Data Factory. Použijte naše ukázka: kopírování dat do a z Azure Blob Storage a Azure SQL Database."
keywords: "data objektu BLOB, kopírovat objekt blob systému azure"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: bec8160f-5e07-47e4-8ee1-ebb14cfb805d
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jingwang
ms.openlocfilehash: 2cf955b52010869a4e753c441e17bdd32fd2e63d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="copy-data-to-or-from-azure-blob-storage-using-azure-data-factory"></a><span data-ttu-id="a169e-105">Kopírovat data do nebo z Azure Blob Storage pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="a169e-105">Copy data to or from Azure Blob Storage using Azure Data Factory</span></span>
<span data-ttu-id="a169e-106">Tento článek vysvětluje, jak pomocí aktivity kopírování v Azure Data Factory ke zkopírování dat do a z Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="a169e-106">This article explains how to use the Copy Activity in Azure Data Factory to copy data to and from Azure Blob Storage.</span></span> <span data-ttu-id="a169e-107">Vychází [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="a169e-107">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

## <a name="overview"></a><span data-ttu-id="a169e-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="a169e-108">Overview</span></span>
<span data-ttu-id="a169e-109">Data můžete zkopírovat z jakékoli podporované zdrojové úložiště dat do úložiště objektů Blob Azure nebo z úložiště objektů Blob Azure do úložiště dat žádné podporované jímky.</span><span class="sxs-lookup"><span data-stu-id="a169e-109">You can copy data from any supported source data store to Azure Blob Storage or from Azure Blob Storage to any supported sink data store.</span></span> <span data-ttu-id="a169e-110">Následující tabulka obsahuje seznam úložiště dat, které jsou podporované jako zdroje nebo jímky pomocí aktivity kopírování.</span><span class="sxs-lookup"><span data-stu-id="a169e-110">The following table provides a list of data stores supported as sources or sinks by the copy activity.</span></span> <span data-ttu-id="a169e-111">Například můžete přesunout data **z** databázi systému SQL Server nebo Azure SQL database **k** Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="a169e-111">For example, you can move data **from** a SQL Server database or an Azure SQL database **to** an Azure blob storage.</span></span> <span data-ttu-id="a169e-112">A může kopírovat data **z** úložiště objektů blob Azure **k** Azure SQL Data Warehouse nebo kolekci Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="a169e-112">And, you can copy data **from** Azure blob storage **to** an Azure SQL Data Warehouse or an Azure Cosmos DB collection.</span></span> 

## <a name="supported-scenarios"></a><span data-ttu-id="a169e-113">Podporované scénáře</span><span class="sxs-lookup"><span data-stu-id="a169e-113">Supported scenarios</span></span>
<span data-ttu-id="a169e-114">Může kopírovat data **z Azure Blob Storage** ukládá do následující data:</span><span class="sxs-lookup"><span data-stu-id="a169e-114">You can copy data **from Azure Blob Storage** to the following data stores:</span></span>

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="a169e-115">Může kopírovat data z následujících datových úložišť **do úložiště objektů Blob Azure**:</span><span class="sxs-lookup"><span data-stu-id="a169e-115">You can copy data from the following data stores **to Azure Blob Storage**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]
 
> [!IMPORTANT]
> <span data-ttu-id="a169e-116">Aktivita kopírování podporuje kopírování dat z/do úložiště objektů Blob v horké nebo nástrojů i pro obecné účely účty Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="a169e-116">Copy Activity supports copying data from/to both general-purpose Azure Storage accounts and Hot/Cool Blob storage.</span></span> <span data-ttu-id="a169e-117">Aktivita podporuje **čtení z bloku, připojte, nebo objekty BLOB stránky**, ale podporuje **zápis do pouze objekty BLOB bloků**.</span><span class="sxs-lookup"><span data-stu-id="a169e-117">The activity supports **reading from block, append, or page blobs**, but supports **writing to only block blobs**.</span></span> <span data-ttu-id="a169e-118">Azure Storage úrovně Premium není podporován jako jímka, protože je zálohovaný díky objekty BLOB stránky.</span><span class="sxs-lookup"><span data-stu-id="a169e-118">Azure Premium Storage is not supported as a sink because it is backed by page blobs.</span></span>
> 
> <span data-ttu-id="a169e-119">Aktivita kopírování nedojde k odstranění dat ze zdroje po dat byl úspěšně zkopírován do cílové.</span><span class="sxs-lookup"><span data-stu-id="a169e-119">Copy Activity does not delete data from the source after the data is successfully copied to the destination.</span></span> <span data-ttu-id="a169e-120">Pokud potřebujete odstranit zdroj dat po úspěšné kopie, vytvořte [vlastní aktivity](data-factory-use-custom-activities.md) k odstranění dat a použijte aktivitu v kanálu.</span><span class="sxs-lookup"><span data-stu-id="a169e-120">If you need to delete source data after a successful copy, create a [custom activity](data-factory-use-custom-activities.md) to delete the data and use the activity in the pipeline.</span></span> <span data-ttu-id="a169e-121">Příklad, naleznete v části [odstranění objektů blob nebo složky ukázce na Githubu](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/DeleteBlobFileFolderCustomActivity).</span><span class="sxs-lookup"><span data-stu-id="a169e-121">For an example, see the [Delete blob or folder sample on GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/DeleteBlobFileFolderCustomActivity).</span></span> 

## <a name="get-started"></a><span data-ttu-id="a169e-122">Začínáme</span><span class="sxs-lookup"><span data-stu-id="a169e-122">Get started</span></span>
<span data-ttu-id="a169e-123">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z Azure Blob Storage pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a169e-123">You can create a pipeline with a copy activity that moves data to/from an Azure Blob Storage by using different tools/APIs.</span></span>

<span data-ttu-id="a169e-124">Nejjednodušší způsob, jak vytvořit kanál je použití **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="a169e-124">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="a169e-125">Tento článek má [návod](#walkthrough-use-copy-wizard-to-copy-data-tofrom-blob-storage) pro vytvoření kanálu pro zkopírování dat z umístění úložiště objektů Blob Azure do jiného umístění úložiště objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="a169e-125">This article has a [walkthrough](#walkthrough-use-copy-wizard-to-copy-data-tofrom-blob-storage) for creating a pipeline to copy data from an Azure Blob Storage location  to another Azure Blob Storage location.</span></span> <span data-ttu-id="a169e-126">Kurz týkající se vytváření kanál ke zkopírování dat z Azure Blob Storage do Azure SQL Database, najdete v části [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="a169e-126">For a tutorial on creating a pipeline to copy data from an Azure Blob Storage to Azure SQL Database, see [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md).</span></span>

<span data-ttu-id="a169e-127">Tyto nástroje můžete také použít k vytvoření kanálu: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru**, **.NET API**, a **REST API**.</span><span class="sxs-lookup"><span data-stu-id="a169e-127">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="a169e-128">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="a169e-128">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span>

<span data-ttu-id="a169e-129">Jestli používáte nástroje nebo rozhraní API, je třeba provést následující kroky k vytvoření kanálu, který přesouvá data ze zdrojového úložiště dat do úložiště dat podřízený:</span><span class="sxs-lookup"><span data-stu-id="a169e-129">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="a169e-130">Vytvoření **objekt pro vytváření dat**.</span><span class="sxs-lookup"><span data-stu-id="a169e-130">Create a **data factory**.</span></span> <span data-ttu-id="a169e-131">Objekt pro vytváření dat může obsahovat jeden nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="a169e-131">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="a169e-132">Vytvoření **propojené služby** propojení vstupní a výstupní data ukládá do data factory.</span><span class="sxs-lookup"><span data-stu-id="a169e-132">Create **linked services** to link input and output data stores to your data factory.</span></span> <span data-ttu-id="a169e-133">Pokud jsou kopírování dat z Azure blob storage do Azure SQL database, například vytvoříte dvě propojené služby pro vytváření dat. propojení účtu úložiště Azure a Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="a169e-133">For example, if you are copying data from an Azure blob storage to an Azure SQL database, you create two linked services to link your Azure storage account and Azure SQL database to your data factory.</span></span> <span data-ttu-id="a169e-134">Vlastnosti propojené služby, které jsou specifické pro Azure Blob Storage, najdete v části [propojené vlastnosti služby](#linked-service-properties) části.</span><span class="sxs-lookup"><span data-stu-id="a169e-134">For linked service properties that are specific to Azure Blob Storage, see [linked service properties](#linked-service-properties) section.</span></span> 
2. <span data-ttu-id="a169e-135">Vytvoření **datové sady** představují vstupní a výstupní data pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="a169e-135">Create **datasets** to represent input and output data for the copy operation.</span></span> <span data-ttu-id="a169e-136">V příkladu uvedených v posledním kroku vytvoříte datové sady a zadat kontejner objektů blob a složky, která obsahuje vstupní data.</span><span class="sxs-lookup"><span data-stu-id="a169e-136">In the example mentioned in the last step, you create a dataset to specify the blob container and folder that contains the input data.</span></span> <span data-ttu-id="a169e-137">A vytvořte jinou datovou sadu, která zadejte tabulku SQL ve službě Azure SQL database, který obsahuje data zkopírovat z úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="a169e-137">And, you create another dataset to specify the SQL table in the Azure SQL database that holds the data copied from the blob storage.</span></span> <span data-ttu-id="a169e-138">Vlastnosti datové sady, které jsou specifické pro Azure Blob Storage, najdete v části [vlastnosti datové sady](#dataset-properties) části.</span><span class="sxs-lookup"><span data-stu-id="a169e-138">For dataset properties that are specific to Azure Blob Storage, see [dataset properties](#dataset-properties) section.</span></span>
3. <span data-ttu-id="a169e-139">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="a169e-139">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="a169e-140">V příkladu již bylo zmíněno dříve použijete BlobSource jako zdroj a SqlSink jako jímku pro aktivitu kopírování.</span><span class="sxs-lookup"><span data-stu-id="a169e-140">In the example mentioned earlier, you use BlobSource as a source and SqlSink as a sink for the copy activity.</span></span> <span data-ttu-id="a169e-141">Podobně pokud kopírujete z databáze SQL Azure do Azure Blob Storage, můžete použít SqlSource a BlobSink v aktivitě kopírování.</span><span class="sxs-lookup"><span data-stu-id="a169e-141">Similarly, if you are copying from Azure SQL Database to Azure Blob Storage, you use SqlSource and BlobSink in the copy activity.</span></span> <span data-ttu-id="a169e-142">Kopírovat vlastnosti aktivity, které jsou specifické pro Azure Blob Storage, najdete v části [zkopírovat vlastnosti aktivity](#copy-activity-properties) části.</span><span class="sxs-lookup"><span data-stu-id="a169e-142">For copy activity properties that are specific to Azure Blob Storage, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="a169e-143">Podrobnosti o tom, jak používat úložiště dat jako zdroj nebo jímka klikněte na odkaz v předchozí části pro data store.</span><span class="sxs-lookup"><span data-stu-id="a169e-143">For details on how to use a data store as a source or a sink, click the link in the previous section for your data store.</span></span>  

<span data-ttu-id="a169e-144">Když použijete průvodce, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál).</span><span class="sxs-lookup"><span data-stu-id="a169e-144">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="a169e-145">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), definujete tyto entity služby Data Factory pomocí formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="a169e-145">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="a169e-146">Ukázky s definicemi JSON entit služby Data Factory, které se používají ke zkopírování dat do/z Azure Blob Storage, najdete v části [JSON příklady](#json-examples-for-copying-data-to-and-from-blob-storage  ) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="a169e-146">For samples with JSON definitions for Data Factory entities that are used to copy data to/from an Azure Blob Storage, see [JSON examples](#json-examples-for-copying-data-to-and-from-blob-storage  ) section of this article.</span></span>

<span data-ttu-id="a169e-147">Následující části obsahují podrobnosti o vlastnostech formátu JSON, které slouží k určení entit služby Data Factory konkrétní do úložiště objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="a169e-147">The following sections provide details about JSON properties that are used to define Data Factory entities specific to Azure Blob Storage.</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="a169e-148">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="a169e-148">Linked service properties</span></span>
<span data-ttu-id="a169e-149">Existují dva typy propojené služby, které můžete použít k propojení Azure Storage do Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="a169e-149">There are two types of linked services you can use to link an Azure Storage to an Azure data factory.</span></span> <span data-ttu-id="a169e-150">Jsou: **azurestorage** propojená služba a **AzureStorageSas** propojené služby.</span><span class="sxs-lookup"><span data-stu-id="a169e-150">They are: **AzureStorage** linked service and **AzureStorageSas** linked service.</span></span> <span data-ttu-id="a169e-151">Propojenou službu úložiště Azure poskytuje objekt pro vytváření dat s globálním přístupem k úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="a169e-151">The Azure Storage linked service provides the data factory with global access to the Azure Storage.</span></span> <span data-ttu-id="a169e-152">Zatímco úložiště SAS Azure (sdíleného přístupového podpisu) propojená služba poskytuje objekt pro vytváření dat s přístupem omezený nebo časově vázaných do úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="a169e-152">Whereas, The Azure Storage SAS (Shared Access Signature) linked service provides the data factory with restricted/time-bound access to the Azure Storage.</span></span> <span data-ttu-id="a169e-153">Nejsou žádné další rozdíly mezi tyto dvě propojené služby.</span><span class="sxs-lookup"><span data-stu-id="a169e-153">There are no other differences between these two linked services.</span></span> <span data-ttu-id="a169e-154">Zvolte propojené služby, která vyhovuje vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="a169e-154">Choose the linked service that suits your needs.</span></span> <span data-ttu-id="a169e-155">Následující části obsahují další podrobnosti na tyto dvě propojené služby.</span><span class="sxs-lookup"><span data-stu-id="a169e-155">The following sections provide more details on these two linked services.</span></span>

[!INCLUDE [data-factory-azure-storage-linked-services](../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="dataset-properties"></a><span data-ttu-id="a169e-156">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="a169e-156">Dataset properties</span></span>
<span data-ttu-id="a169e-157">Pokud chcete zadat datové sady představují vstupních nebo výstupních dat v Azure Blob Storage, nastavte vlastnost type datové sady, která: **AzureBlob**.</span><span class="sxs-lookup"><span data-stu-id="a169e-157">To specify a dataset to represent input or output data in an Azure Blob Storage, you set the type property of the dataset to: **AzureBlob**.</span></span> <span data-ttu-id="a169e-158">Nastavte **linkedServiceName** vlastnosti datové sady, která název Azure Storage nebo Azure úložiště SAS propojené služby.</span><span class="sxs-lookup"><span data-stu-id="a169e-158">Set the **linkedServiceName** property of the dataset to the name of the Azure Storage or Azure Storage SAS linked service.</span></span>  <span data-ttu-id="a169e-159">Zadejte typ vlastnosti datové sady **kontejner objektů blob** a **složky** ve službě blob storage.</span><span class="sxs-lookup"><span data-stu-id="a169e-159">The type properties of the dataset specify the **blob container** and the **folder** in the blob storage.</span></span>

<span data-ttu-id="a169e-160">Úplný seznam části JSON & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v článku [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="a169e-160">For a full list of JSON sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="a169e-161">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="a169e-161">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="a169e-162">Objekt pro vytváření dat podporuje následující hodnoty typu kompatibilní se specifikací CLS .NET na základě poskytnutí informací o typu "struktury" zdroje dat schématu na čtení jako objekt blob systému Azure: Int16, Int32, Int64, jeden, Double, Decimal, Byte [], Bool, řetězec, Guid, Datetime, Datetimeoffset, Timespan.</span><span class="sxs-lookup"><span data-stu-id="a169e-162">Data factory supports the following CLS-compliant .NET based type values for providing type information in “structure” for schema-on-read data sources like Azure blob: Int16, Int32, Int64, Single, Double, Decimal, Byte[], Bool, String, Guid, Datetime, Datetimeoffset, Timespan.</span></span> <span data-ttu-id="a169e-163">Objekt pro vytváření dat automaticky provede převody typů, při přesouvání dat ze zdrojového úložiště dat do úložiště dat jímky.</span><span class="sxs-lookup"><span data-stu-id="a169e-163">Data Factory automatically performs type conversions when moving data from a source data store to a sink data store.</span></span>

<span data-ttu-id="a169e-164">**Rámci typeProperties** oddílu se liší pro jednotlivé typy datovou sadu a informace o umístění, formátování atd, dat v úložišti.</span><span class="sxs-lookup"><span data-stu-id="a169e-164">The **typeProperties** section is different for each type of dataset and provides information about the location, format etc., of the data in the data store.</span></span> <span data-ttu-id="a169e-165">Rámci typeProperties část datové sady typ **AzureBlob** datová sada má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="a169e-165">The typeProperties section for dataset of type **AzureBlob** dataset has the following properties:</span></span>

| <span data-ttu-id="a169e-166">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="a169e-166">Property</span></span> | <span data-ttu-id="a169e-167">Popis</span><span class="sxs-lookup"><span data-stu-id="a169e-167">Description</span></span> | <span data-ttu-id="a169e-168">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="a169e-168">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a169e-169">folderPath</span><span class="sxs-lookup"><span data-stu-id="a169e-169">folderPath</span></span> |<span data-ttu-id="a169e-170">Cesta ke kontejneru a složce v úložišti objektů blob.</span><span class="sxs-lookup"><span data-stu-id="a169e-170">Path to the container and folder in the blob storage.</span></span> <span data-ttu-id="a169e-171">Příklad: myblobcontainer\myblobfolder\\</span><span class="sxs-lookup"><span data-stu-id="a169e-171">Example: myblobcontainer\myblobfolder\\</span></span> |<span data-ttu-id="a169e-172">Ano</span><span class="sxs-lookup"><span data-stu-id="a169e-172">Yes</span></span> |
| <span data-ttu-id="a169e-173">fileName</span><span class="sxs-lookup"><span data-stu-id="a169e-173">fileName</span></span> |<span data-ttu-id="a169e-174">Název objektu blob.</span><span class="sxs-lookup"><span data-stu-id="a169e-174">Name of the blob.</span></span> <span data-ttu-id="a169e-175">Název souboru je volitelné a velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="a169e-175">fileName is optional and case-sensitive.</span></span><br/><br/><span data-ttu-id="a169e-176">Pokud zadáte název souboru, na konkrétní objekt Blob funguje aktivitu (včetně kopie).</span><span class="sxs-lookup"><span data-stu-id="a169e-176">If you specify a filename, the activity (including Copy) works on the specific Blob.</span></span><br/><br/><span data-ttu-id="a169e-177">Pokud není zadán název souboru, zahrnuje kopírování všech objektů BLOB v folderPath pro vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="a169e-177">When fileName is not specified, Copy includes all Blobs in the folderPath for input dataset.</span></span><br/><br/><span data-ttu-id="a169e-178">Když **fileName** pro datovou sadu výstupů není zadána a **preserveHierarchy** není zadané v aktivity podřízený název vygenerovaný soubor bude v následujícím tento formát: Data.<Guid>. TXT (například:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="a169e-178">When **fileName** is not specified for an output dataset and **preserveHierarchy** is not specified in activity sink, the name of the generated file would be in the following this format: Data.<Guid>.txt (for example: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="a169e-179">Ne</span><span class="sxs-lookup"><span data-stu-id="a169e-179">No</span></span> |
| <span data-ttu-id="a169e-180">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="a169e-180">partitionedBy</span></span> |<span data-ttu-id="a169e-181">partitionedBy vlastnost je volitelná.</span><span class="sxs-lookup"><span data-stu-id="a169e-181">partitionedBy is an optional property.</span></span> <span data-ttu-id="a169e-182">Můžete ji k určení dynamické folderPath a název souboru pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="a169e-182">You can use it to specify a dynamic folderPath and filename for time series data.</span></span> <span data-ttu-id="a169e-183">Například folderPath lze nastavit parametry pro každou hodinu data.</span><span class="sxs-lookup"><span data-stu-id="a169e-183">For example, folderPath can be parameterized for every hour of data.</span></span> <span data-ttu-id="a169e-184">Najdete v článku [pomocí části vlastnost partitionedBy](#using-partitionedBy-property) podrobnosti a příklady.</span><span class="sxs-lookup"><span data-stu-id="a169e-184">See the [Using partitionedBy property section](#using-partitionedBy-property) for details and examples.</span></span> |<span data-ttu-id="a169e-185">Ne</span><span class="sxs-lookup"><span data-stu-id="a169e-185">No</span></span> |
| <span data-ttu-id="a169e-186">Formát</span><span class="sxs-lookup"><span data-stu-id="a169e-186">format</span></span> | <span data-ttu-id="a169e-187">Jsou podporovány následující typy formátu: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="a169e-187">The following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="a169e-188">Nastavte **typ** vlastnost pod formát na jednu z těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="a169e-188">Set the **type** property under format to one of these values.</span></span> <span data-ttu-id="a169e-189">Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly.</span><span class="sxs-lookup"><span data-stu-id="a169e-189">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="a169e-190">Pokud chcete **zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu v obou definice vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="a169e-190">If you want to **copy files as-is** between file-based stores (binary copy), skip the format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="a169e-191">Ne</span><span class="sxs-lookup"><span data-stu-id="a169e-191">No</span></span> |
| <span data-ttu-id="a169e-192">Komprese</span><span class="sxs-lookup"><span data-stu-id="a169e-192">compression</span></span> | <span data-ttu-id="a169e-193">Zadejte typ a úroveň komprese pro data.</span><span class="sxs-lookup"><span data-stu-id="a169e-193">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="a169e-194">Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="a169e-194">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="a169e-195">Jsou podporované úrovně: **Optimal** a **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="a169e-195">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="a169e-196">Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="a169e-196">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="a169e-197">Ne</span><span class="sxs-lookup"><span data-stu-id="a169e-197">No</span></span> |

### <a name="using-partitionedby-property"></a><span data-ttu-id="a169e-198">Pomocí vlastnost partitionedBy</span><span class="sxs-lookup"><span data-stu-id="a169e-198">Using partitionedBy property</span></span>
<span data-ttu-id="a169e-199">Jak je uvedeno v předchozí části, můžete zadat dynamické folderPath a název souboru pro data časové řady s **partitionedBy** vlastnost [funkce pro vytváření dat a systémové proměnné](data-factory-functions-variables.md).</span><span class="sxs-lookup"><span data-stu-id="a169e-199">As mentioned in the previous section, you can specify a dynamic folderPath and filename for time series data with the **partitionedBy** property, [Data Factory functions, and the system variables](data-factory-functions-variables.md).</span></span>

<span data-ttu-id="a169e-200">Další informace o datové sady času řady, plánování a řezy najdete v tématu [vytváření datových sad](data-factory-create-datasets.md) a [plánování a provádění](data-factory-scheduling-and-execution.md) články.</span><span class="sxs-lookup"><span data-stu-id="a169e-200">For more information on time series datasets, scheduling, and slices, see [Creating Datasets](data-factory-create-datasets.md) and [Scheduling & Execution](data-factory-scheduling-and-execution.md) articles.</span></span>

#### <a name="sample-1"></a><span data-ttu-id="a169e-201">Ukázka 1</span><span class="sxs-lookup"><span data-stu-id="a169e-201">Sample 1</span></span>

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

<span data-ttu-id="a169e-202">V tomto příkladu {řez} se nahradí hodnotu objektu pro vytváření dat systému proměnné SliceStart ve formátu (YYYYMMDDHH) zadán.</span><span class="sxs-lookup"><span data-stu-id="a169e-202">In this example, {Slice} is replaced with the value of Data Factory system variable SliceStart in the format (YYYYMMDDHH) specified.</span></span> <span data-ttu-id="a169e-203">Vlastnosti SliceStart odkazuje na spuštění řezu.</span><span class="sxs-lookup"><span data-stu-id="a169e-203">The SliceStart refers to start time of the slice.</span></span> <span data-ttu-id="a169e-204">FolderPath se liší pro každý řez.</span><span class="sxs-lookup"><span data-stu-id="a169e-204">The folderPath is different for each slice.</span></span> <span data-ttu-id="a169e-205">Příklad: wikidatagateway/wikisampledataout/2014100103 nebo wikidatagateway/wikisampledataout/2014100104</span><span class="sxs-lookup"><span data-stu-id="a169e-205">For example: wikidatagateway/wikisampledataout/2014100103 or wikidatagateway/wikisampledataout/2014100104</span></span>

#### <a name="sample-2"></a><span data-ttu-id="a169e-206">Ukázka 2</span><span class="sxs-lookup"><span data-stu-id="a169e-206">Sample 2</span></span>

```json
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```

<span data-ttu-id="a169e-207">V tomto příkladu jsou extrahován rok, měsíc, den a čas SliceStart do samostatné proměnné, které jsou používány folderPath a název vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="a169e-207">In this example, year, month, day, and time of SliceStart are extracted into separate variables that are used by folderPath and fileName properties.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="a169e-208">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="a169e-208">Copy activity properties</span></span>
<span data-ttu-id="a169e-209">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="a169e-209">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="a169e-210">Vlastnosti, například název, popis, vstupní a výstupní datové sady a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="a169e-210">Properties such as name, description, input and output datasets, and policies are available for all types of activities.</span></span> <span data-ttu-id="a169e-211">Vzhledem k tomu, vlastnosti dostupné ve **rámci typeProperties** části aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="a169e-211">Whereas, properties available in the **typeProperties** section of the activity vary with each activity type.</span></span> <span data-ttu-id="a169e-212">Pro aktivitu kopírování budou lišit v závislosti na typech zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="a169e-212">For Copy activity, they vary depending on the types of sources and sinks.</span></span> <span data-ttu-id="a169e-213">Pokud přesouváte data z Azure Blob Storage, nastavíte typ zdroje v aktivitě kopírování do **BlobSource**.</span><span class="sxs-lookup"><span data-stu-id="a169e-213">If you are moving data from an Azure Blob Storage, you set the source type in the copy activity to **BlobSource**.</span></span> <span data-ttu-id="a169e-214">Podobně pokud přesouváte data do Azure Blob Storage, nastavíte typ jímky v aktivitě kopírování do **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="a169e-214">Similarly, if you are moving data to an Azure Blob Storage, you set the sink type in the copy activity to **BlobSink**.</span></span> <span data-ttu-id="a169e-215">Tato část obsahuje seznam vlastností, které jsou podporované BlobSource a BlobSink.</span><span class="sxs-lookup"><span data-stu-id="a169e-215">This section provides a list of properties supported by BlobSource and BlobSink.</span></span>

<span data-ttu-id="a169e-216">**BlobSource** podporuje následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="a169e-216">**BlobSource** supports the following properties in the **typeProperties** section:</span></span>

| <span data-ttu-id="a169e-217">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="a169e-217">Property</span></span> | <span data-ttu-id="a169e-218">Popis</span><span class="sxs-lookup"><span data-stu-id="a169e-218">Description</span></span> | <span data-ttu-id="a169e-219">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="a169e-219">Allowed values</span></span> | <span data-ttu-id="a169e-220">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="a169e-220">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a169e-221">Rekurzivní</span><span class="sxs-lookup"><span data-stu-id="a169e-221">recursive</span></span> |<span data-ttu-id="a169e-222">Označuje, zda je data načíst rekurzivně z dílčí složky nebo pouze do zadané složky.</span><span class="sxs-lookup"><span data-stu-id="a169e-222">Indicates whether the data is read recursively from the sub folders or only from the specified folder.</span></span> |<span data-ttu-id="a169e-223">True (výchozí hodnota), False.</span><span class="sxs-lookup"><span data-stu-id="a169e-223">True (default value), False</span></span> |<span data-ttu-id="a169e-224">Ne</span><span class="sxs-lookup"><span data-stu-id="a169e-224">No</span></span> |

<span data-ttu-id="a169e-225">**BlobSink** podporuje následující vlastnosti **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="a169e-225">**BlobSink** supports the following properties **typeProperties** section:</span></span>

| <span data-ttu-id="a169e-226">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="a169e-226">Property</span></span> | <span data-ttu-id="a169e-227">Popis</span><span class="sxs-lookup"><span data-stu-id="a169e-227">Description</span></span> | <span data-ttu-id="a169e-228">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="a169e-228">Allowed values</span></span> | <span data-ttu-id="a169e-229">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="a169e-229">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a169e-230">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="a169e-230">copyBehavior</span></span> |<span data-ttu-id="a169e-231">Definuje chování kopie, pokud je zdroj BlobSource nebo systému souborů.</span><span class="sxs-lookup"><span data-stu-id="a169e-231">Defines the copy behavior when the source is BlobSource or FileSystem.</span></span> |<span data-ttu-id="a169e-232"><b>PreserveHierarchy</b>: zachovává hierarchii souborů v cílové složce.</span><span class="sxs-lookup"><span data-stu-id="a169e-232"><b>PreserveHierarchy</b>: preserves the file hierarchy in the target folder.</span></span> <span data-ttu-id="a169e-233">Relativní cesta zdrojového souboru do zdrojové složky je stejný jako relativní cestu k souboru cíl k cílové složce.</span><span class="sxs-lookup"><span data-stu-id="a169e-233">The relative path of source file to source folder is identical to the relative path of target file to target folder.</span></span><br/><br/><span data-ttu-id="a169e-234"><b>FlattenHierarchy</b>: všechny soubory ze zdrojové složky jsou v první úroveň cílové složce.</span><span class="sxs-lookup"><span data-stu-id="a169e-234"><b>FlattenHierarchy</b>: all files from the source folder are in the first level of target folder.</span></span> <span data-ttu-id="a169e-235">Cílové soubory mít název automaticky generovány.</span><span class="sxs-lookup"><span data-stu-id="a169e-235">The target files have auto generated name.</span></span> <br/><br/><span data-ttu-id="a169e-236"><b>MergeFiles</b>: sloučí všechny soubory ze zdrojové složky pro jeden soubor.</span><span class="sxs-lookup"><span data-stu-id="a169e-236"><b>MergeFiles</b>: merges all files from the source folder to one file.</span></span> <span data-ttu-id="a169e-237">Pokud je zadán název souboru nebo objekt Blob, název souboru sloučené by být zadaný název; jinak by automaticky generovaný soubor název.</span><span class="sxs-lookup"><span data-stu-id="a169e-237">If the File/Blob Name is specified, the merged file name would be the specified name; otherwise, would be auto-generated file name.</span></span> |<span data-ttu-id="a169e-238">Ne</span><span class="sxs-lookup"><span data-stu-id="a169e-238">No</span></span> |

<span data-ttu-id="a169e-239">**BlobSource** také podporuje tyto dvě vlastnosti z důvodu zpětné kompatibility.</span><span class="sxs-lookup"><span data-stu-id="a169e-239">**BlobSource** also supports these two properties for backward compatibility.</span></span>

* <span data-ttu-id="a169e-240">**treatEmptyAsNull**: Určuje, jestli má hodnotu null nebo prázdný řetězec považovat za hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="a169e-240">**treatEmptyAsNull**: Specifies whether to treat null or empty string as null value.</span></span>
* <span data-ttu-id="a169e-241">**skipHeaderLineCount** -Určuje, kolik řádků potřebovat přeskočen.</span><span class="sxs-lookup"><span data-stu-id="a169e-241">**skipHeaderLineCount** - Specifies how many lines need be skipped.</span></span> <span data-ttu-id="a169e-242">Vztahuje se pouze pokud vstupní datové sady používá TextFormat.</span><span class="sxs-lookup"><span data-stu-id="a169e-242">It is applicable only when input dataset is using TextFormat.</span></span>

<span data-ttu-id="a169e-243">Podobně **BlobSink** podporuje následující vlastnost z důvodu zpětné kompatibility.</span><span class="sxs-lookup"><span data-stu-id="a169e-243">Similarly, **BlobSink** supports the following property for backward compatibility.</span></span>

* <span data-ttu-id="a169e-244">**blobWriterAddHeader**: Určuje, zda chcete přidat hlavičku definice sloupců při zápisu do výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="a169e-244">**blobWriterAddHeader**: Specifies whether to add a header of column definitions while writing to an output dataset.</span></span>

<span data-ttu-id="a169e-245">Datové sady teď podporují následující vlastnosti, které implementují stejnou funkci: **treatEmptyAsNull**, **skipLineCount**, **firstRowAsHeader**.</span><span class="sxs-lookup"><span data-stu-id="a169e-245">Datasets now support the following properties that implement the same functionality: **treatEmptyAsNull**, **skipLineCount**, **firstRowAsHeader**.</span></span>

<span data-ttu-id="a169e-246">Následující tabulka obsahuje informace o použití nové vlastnosti datové sady místo tyto vlastnosti Zdroj/jímka objektů blob.</span><span class="sxs-lookup"><span data-stu-id="a169e-246">The following table provides guidance on using the new dataset properties in place of these blob source/sink properties.</span></span>

| <span data-ttu-id="a169e-247">Vlastnost aktivity kopírování</span><span class="sxs-lookup"><span data-stu-id="a169e-247">Copy Activity property</span></span> | <span data-ttu-id="a169e-248">Vlastnost DataSet</span><span class="sxs-lookup"><span data-stu-id="a169e-248">Dataset property</span></span> |
|:--- |:--- |
| <span data-ttu-id="a169e-249">skipHeaderLineCount na BlobSource</span><span class="sxs-lookup"><span data-stu-id="a169e-249">skipHeaderLineCount on BlobSource</span></span> |<span data-ttu-id="a169e-250">skipLineCount a firstRowAsHeader.</span><span class="sxs-lookup"><span data-stu-id="a169e-250">skipLineCount and firstRowAsHeader.</span></span> <span data-ttu-id="a169e-251">Řádky jsou přeskočeny první a pak je pro čtení první řádek jako záhlaví.</span><span class="sxs-lookup"><span data-stu-id="a169e-251">Lines are skipped first and then the first row is read as a header.</span></span> |
| <span data-ttu-id="a169e-252">treatEmptyAsNull na BlobSource</span><span class="sxs-lookup"><span data-stu-id="a169e-252">treatEmptyAsNull on BlobSource</span></span> |<span data-ttu-id="a169e-253">treatEmptyAsNull na vstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="a169e-253">treatEmptyAsNull on input dataset</span></span> |
| <span data-ttu-id="a169e-254">blobWriterAddHeader na BlobSink</span><span class="sxs-lookup"><span data-stu-id="a169e-254">blobWriterAddHeader on BlobSink</span></span> |<span data-ttu-id="a169e-255">firstRowAsHeader na výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="a169e-255">firstRowAsHeader on output dataset</span></span> |

<span data-ttu-id="a169e-256">V tématu [zadání TextFormat](data-factory-supported-file-and-compression-formats.md#text-format) části Podrobné informace o těchto vlastností.</span><span class="sxs-lookup"><span data-stu-id="a169e-256">See [Specifying TextFormat](data-factory-supported-file-and-compression-formats.md#text-format) section for detailed information on these properties.</span></span>    

### <a name="recursive-and-copybehavior-examples"></a><span data-ttu-id="a169e-257">Příklady rekurzivní a copyBehavior</span><span class="sxs-lookup"><span data-stu-id="a169e-257">recursive and copyBehavior examples</span></span>
<span data-ttu-id="a169e-258">Tato část popisuje jejich výsledné chování pro různé kombinace hodnot rekurzivní a copyBehavior operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="a169e-258">This section describes the resulting behavior of the Copy operation for different combinations of recursive and copyBehavior values.</span></span>

| <span data-ttu-id="a169e-259">Rekurzivní</span><span class="sxs-lookup"><span data-stu-id="a169e-259">recursive</span></span> | <span data-ttu-id="a169e-260">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="a169e-260">copyBehavior</span></span> | <span data-ttu-id="a169e-261">Výsledné chování</span><span class="sxs-lookup"><span data-stu-id="a169e-261">Resulting behavior</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a169e-262">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="a169e-262">true</span></span> |<span data-ttu-id="a169e-263">preserveHierarchy</span><span class="sxs-lookup"><span data-stu-id="a169e-263">preserveHierarchy</span></span> |<span data-ttu-id="a169e-264">Pro zdrojové složky složku1 s následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="a169e-264">For a source folder Folder1 with the following structure:</span></span> <br/><br/><span data-ttu-id="a169e-265">Složku1</span><span class="sxs-lookup"><span data-stu-id="a169e-265">Folder1</span></span><br/><span data-ttu-id="a169e-266">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="a169e-266">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="a169e-267">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="a169e-267">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="a169e-268">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="a169e-268">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="a169e-269">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="a169e-269">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="a169e-270">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="a169e-270">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="a169e-271">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="a169e-271">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="a169e-272">cílové složky složku1 je vytvořena s stejná struktura jako zdroj</span><span class="sxs-lookup"><span data-stu-id="a169e-272">the target folder Folder1 is created with the same structure as the source</span></span><br/><br/><span data-ttu-id="a169e-273">Složku1</span><span class="sxs-lookup"><span data-stu-id="a169e-273">Folder1</span></span><br/><span data-ttu-id="a169e-274">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="a169e-274">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="a169e-275">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="a169e-275">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="a169e-276">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="a169e-276">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="a169e-277">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="a169e-277">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="a169e-278">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="a169e-278">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="a169e-279">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.</span><span class="sxs-lookup"><span data-stu-id="a169e-279">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.</span></span> |
| <span data-ttu-id="a169e-280">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="a169e-280">true</span></span> |<span data-ttu-id="a169e-281">flattenHierarchy</span><span class="sxs-lookup"><span data-stu-id="a169e-281">flattenHierarchy</span></span> |<span data-ttu-id="a169e-282">Pro zdrojové složky složku1 s následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="a169e-282">For a source folder Folder1 with the following structure:</span></span> <br/><br/><span data-ttu-id="a169e-283">Složku1</span><span class="sxs-lookup"><span data-stu-id="a169e-283">Folder1</span></span><br/><span data-ttu-id="a169e-284">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="a169e-284">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="a169e-285">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="a169e-285">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="a169e-286">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="a169e-286">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="a169e-287">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="a169e-287">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="a169e-288">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="a169e-288">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="a169e-289">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="a169e-289">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="a169e-290">cíl složku1 je vytvořen s následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="a169e-290">the target Folder1 is created with the following structure:</span></span> <br/><br/><span data-ttu-id="a169e-291">Složku1</span><span class="sxs-lookup"><span data-stu-id="a169e-291">Folder1</span></span><br/><span data-ttu-id="a169e-292">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File1</span><span class="sxs-lookup"><span data-stu-id="a169e-292">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File1</span></span><br/><span data-ttu-id="a169e-293">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File2</span><span class="sxs-lookup"><span data-stu-id="a169e-293">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File2</span></span><br/><span data-ttu-id="a169e-294">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název soubor3</span><span class="sxs-lookup"><span data-stu-id="a169e-294">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File3</span></span><br/><span data-ttu-id="a169e-295">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File4</span><span class="sxs-lookup"><span data-stu-id="a169e-295">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File4</span></span><br/><span data-ttu-id="a169e-296">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File5</span><span class="sxs-lookup"><span data-stu-id="a169e-296">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File5</span></span> |
| <span data-ttu-id="a169e-297">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="a169e-297">true</span></span> |<span data-ttu-id="a169e-298">mergeFiles</span><span class="sxs-lookup"><span data-stu-id="a169e-298">mergeFiles</span></span> |<span data-ttu-id="a169e-299">Pro zdrojové složky složku1 s následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="a169e-299">For a source folder Folder1 with the following structure:</span></span> <br/><br/><span data-ttu-id="a169e-300">Složku1</span><span class="sxs-lookup"><span data-stu-id="a169e-300">Folder1</span></span><br/><span data-ttu-id="a169e-301">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="a169e-301">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="a169e-302">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="a169e-302">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="a169e-303">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="a169e-303">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="a169e-304">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="a169e-304">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="a169e-305">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="a169e-305">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="a169e-306">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="a169e-306">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="a169e-307">cíl složku1 je vytvořen s následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="a169e-307">the target Folder1 is created with the following structure:</span></span> <br/><br/><span data-ttu-id="a169e-308">Složku1</span><span class="sxs-lookup"><span data-stu-id="a169e-308">Folder1</span></span><br/><span data-ttu-id="a169e-309">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + soubor3 + File4 + soubor 5 obsah jsou sloučeny do jednoho souboru s názvem automaticky generovaný soubor</span><span class="sxs-lookup"><span data-stu-id="a169e-309">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + File3 + File4 + File 5 contents are merged into one file with auto-generated file name</span></span> |
| <span data-ttu-id="a169e-310">False</span><span class="sxs-lookup"><span data-stu-id="a169e-310">false</span></span> |<span data-ttu-id="a169e-311">preserveHierarchy</span><span class="sxs-lookup"><span data-stu-id="a169e-311">preserveHierarchy</span></span> |<span data-ttu-id="a169e-312">Pro zdrojové složky složku1 s následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="a169e-312">For a source folder Folder1 with the following structure:</span></span> <br/><br/><span data-ttu-id="a169e-313">Složku1</span><span class="sxs-lookup"><span data-stu-id="a169e-313">Folder1</span></span><br/><span data-ttu-id="a169e-314">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="a169e-314">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="a169e-315">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="a169e-315">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="a169e-316">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="a169e-316">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="a169e-317">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="a169e-317">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="a169e-318">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="a169e-318">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="a169e-319">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="a169e-319">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="a169e-320">Vytvoření cílové složky složku1 s následující strukturou</span><span class="sxs-lookup"><span data-stu-id="a169e-320">the target folder Folder1 is created with the following structure</span></span><br/><br/><span data-ttu-id="a169e-321">Složku1</span><span class="sxs-lookup"><span data-stu-id="a169e-321">Folder1</span></span><br/><span data-ttu-id="a169e-322">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="a169e-322">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="a169e-323">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="a169e-323">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><br/><br/><span data-ttu-id="a169e-324">Subfolder1 s soubor3, File4 a File5 nejsou zachyceny.</span><span class="sxs-lookup"><span data-stu-id="a169e-324">Subfolder1 with File3, File4, and File5 are not picked up.</span></span> |
| <span data-ttu-id="a169e-325">False</span><span class="sxs-lookup"><span data-stu-id="a169e-325">false</span></span> |<span data-ttu-id="a169e-326">flattenHierarchy</span><span class="sxs-lookup"><span data-stu-id="a169e-326">flattenHierarchy</span></span> |<span data-ttu-id="a169e-327">Pro zdrojové složky složku1 s následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="a169e-327">For a source folder Folder1 with the following structure:</span></span><br/><br/><span data-ttu-id="a169e-328">Složku1</span><span class="sxs-lookup"><span data-stu-id="a169e-328">Folder1</span></span><br/><span data-ttu-id="a169e-329">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="a169e-329">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="a169e-330">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="a169e-330">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="a169e-331">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="a169e-331">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="a169e-332">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="a169e-332">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="a169e-333">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="a169e-333">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="a169e-334">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="a169e-334">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="a169e-335">Vytvoření cílové složky složku1 s následující strukturou</span><span class="sxs-lookup"><span data-stu-id="a169e-335">the target folder Folder1 is created with the following structure</span></span><br/><br/><span data-ttu-id="a169e-336">Složku1</span><span class="sxs-lookup"><span data-stu-id="a169e-336">Folder1</span></span><br/><span data-ttu-id="a169e-337">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File1</span><span class="sxs-lookup"><span data-stu-id="a169e-337">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File1</span></span><br/><span data-ttu-id="a169e-338">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File2</span><span class="sxs-lookup"><span data-stu-id="a169e-338">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File2</span></span><br/><br/><br/><span data-ttu-id="a169e-339">Subfolder1 s soubor3, File4 a File5 nejsou zachyceny.</span><span class="sxs-lookup"><span data-stu-id="a169e-339">Subfolder1 with File3, File4, and File5 are not picked up.</span></span> |
| <span data-ttu-id="a169e-340">False</span><span class="sxs-lookup"><span data-stu-id="a169e-340">false</span></span> |<span data-ttu-id="a169e-341">mergeFiles</span><span class="sxs-lookup"><span data-stu-id="a169e-341">mergeFiles</span></span> |<span data-ttu-id="a169e-342">Pro zdrojové složky složku1 s následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="a169e-342">For a source folder Folder1 with the following structure:</span></span><br/><br/><span data-ttu-id="a169e-343">Složku1</span><span class="sxs-lookup"><span data-stu-id="a169e-343">Folder1</span></span><br/><span data-ttu-id="a169e-344">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="a169e-344">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="a169e-345">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="a169e-345">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="a169e-346">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="a169e-346">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="a169e-347">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="a169e-347">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="a169e-348">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="a169e-348">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="a169e-349">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="a169e-349">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="a169e-350">Vytvoření cílové složky složku1 s následující strukturou</span><span class="sxs-lookup"><span data-stu-id="a169e-350">the target folder Folder1 is created with the following structure</span></span><br/><br/><span data-ttu-id="a169e-351">Složku1</span><span class="sxs-lookup"><span data-stu-id="a169e-351">Folder1</span></span><br/><span data-ttu-id="a169e-352">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 obsah jsou sloučeny do jednoho souboru s názvem automaticky generovaný soubor.</span><span class="sxs-lookup"><span data-stu-id="a169e-352">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 contents are merged into one file with auto-generated file name.</span></span> <span data-ttu-id="a169e-353">automaticky generovaný název File1</span><span class="sxs-lookup"><span data-stu-id="a169e-353">auto-generated name for File1</span></span><br/><br/><span data-ttu-id="a169e-354">Subfolder1 s soubor3, File4 a File5 nejsou zachyceny.</span><span class="sxs-lookup"><span data-stu-id="a169e-354">Subfolder1 with File3, File4, and File5 are not picked up.</span></span> |

## <a name="walkthrough-use-copy-wizard-to-copy-data-tofrom-blob-storage"></a><span data-ttu-id="a169e-355">Návod: Použití Průvodce kopírováním ke zkopírování dat do nebo z úložiště objektů Blob</span><span class="sxs-lookup"><span data-stu-id="a169e-355">Walkthrough: Use Copy Wizard to copy data to/from Blob Storage</span></span>
<span data-ttu-id="a169e-356">Podívejme se na tom, jak rychle kopírování dat z Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="a169e-356">Let's look at how to quickly copy data to/from an Azure blob storage.</span></span> <span data-ttu-id="a169e-357">V tomto návodu ukládá data zdrojového a cílového typu: Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="a169e-357">In this walkthrough, both source and destination data stores of type: Azure Blob Storage.</span></span> <span data-ttu-id="a169e-358">Kanál v tomto návodu kopíruje data ze složky do jiné složky ve stejném kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="a169e-358">The pipeline in this walkthrough copies data from a folder to another folder in the same blob container.</span></span> <span data-ttu-id="a169e-359">Tento názorný postup je tak, aby zobrazovalo nastavení nebo vlastnosti při používání Blob Storage jako zdroj nebo podřízený záměrně jednoduchá.</span><span class="sxs-lookup"><span data-stu-id="a169e-359">This walkthrough is intentionally simple to show you settings or properties when using Blob Storage as a source or sink.</span></span> 

### <a name="prerequisites"></a><span data-ttu-id="a169e-360">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a169e-360">Prerequisites</span></span>
1. <span data-ttu-id="a169e-361">Vytvoření pro obecné účely **účet úložiště Azure** Pokud již nemáte.</span><span class="sxs-lookup"><span data-stu-id="a169e-361">Create a general-purpose **Azure Storage Account** if you don't have one already.</span></span> <span data-ttu-id="a169e-362">Použít úložiště objektů blob jako obě **zdroj** a **cílové** úložiště dat v tomto návodu.</span><span class="sxs-lookup"><span data-stu-id="a169e-362">You use the blob storage as both **source** and **destination** data store in this walkthrough.</span></span> <span data-ttu-id="a169e-363">Pokud nemáte účet úložiště Azure, najdete v článku [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) najdete v článku kroky k jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="a169e-363">if you don't have an Azure storage account, see the [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) article for steps to create one.</span></span>
2. <span data-ttu-id="a169e-364">Vytvořte kontejner objektů blob s názvem **adfblobconnector** v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="a169e-364">Create a blob container named **adfblobconnector** in the storage account.</span></span> 
4. <span data-ttu-id="a169e-365">Vytvořte složku s názvem **vstupní** v **adfblobconnector** kontejneru.</span><span class="sxs-lookup"><span data-stu-id="a169e-365">Create a folder named **input** in the **adfblobconnector** container.</span></span>
5. <span data-ttu-id="a169e-366">Vytvořte soubor s názvem **emp.txt** s následující obsahu a nahrajte ho do **vstupní** složky pomocí nástrojů, jako například [Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/)</span><span class="sxs-lookup"><span data-stu-id="a169e-366">Create a file named **emp.txt** with the following content and upload it to the **input** folder by using tools such as [Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/)</span></span>
    ```json
    John, Doe
    Jane, Doe
    ```
### <a name="create-the-data-factory"></a><span data-ttu-id="a169e-367">Vytvoření objektu pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="a169e-367">Create the data factory</span></span>
1. <span data-ttu-id="a169e-368">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a169e-368">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="a169e-369">Klikněte na tlačítko **+ nový** v levém horním rohu klikněte na **Intelligence + analýzy**a klikněte na tlačítko **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="a169e-369">Click **+ NEW** from the top-left corner, click **Intelligence + analytics**, and click **Data Factory**.</span></span>
3. <span data-ttu-id="a169e-370">V okně **Nový objekt pro vytváření dat**:</span><span class="sxs-lookup"><span data-stu-id="a169e-370">In the **New data factory** blade:</span></span>   
    1. <span data-ttu-id="a169e-371">Zadejte **ADFBlobConnectorDF** pro **název**.</span><span class="sxs-lookup"><span data-stu-id="a169e-371">Enter **ADFBlobConnectorDF** for the **name**.</span></span> <span data-ttu-id="a169e-372">Název objektu pro vytváření dat Azure musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="a169e-372">The name of the Azure data factory must be globally unique.</span></span> <span data-ttu-id="a169e-373">Pokud se zobrazí chyba: `*Data factory name “ADFBlobConnectorDF” is not available`, změňte název objektu pro vytváření dat (například yournameADFBlobConnectorDF) a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="a169e-373">If you receive the error: `*Data factory name “ADFBlobConnectorDF” is not available`, change the name of the data factory (for example, yournameADFBlobConnectorDF) and try creating again.</span></span> <span data-ttu-id="a169e-374">V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="a169e-374">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>
    2. <span data-ttu-id="a169e-375">Vyberte své **předplatné** Azure.</span><span class="sxs-lookup"><span data-stu-id="a169e-375">Select your Azure **subscription**.</span></span>
    3. <span data-ttu-id="a169e-376">Pro skupinu prostředků, vyberte **použít existující** vyberte existující skupinu prostředků (nebo) vyberte **vytvořit nový** k zadání názvu pro skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="a169e-376">For Resource Group, select **Use existing** to select an existing resource group (or) select **Create new** to enter a name for a resource group.</span></span>
    4. <span data-ttu-id="a169e-377">Vyberte **umístění** pro příslušný objekt pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="a169e-377">Select a **location** for the data factory.</span></span>
    5. <span data-ttu-id="a169e-378">Zaškrtněte políčko **Připnout na řídicí panel** v dolní části okna.</span><span class="sxs-lookup"><span data-stu-id="a169e-378">Select **Pin to dashboard** check box at the bottom of the blade.</span></span>
    6. <span data-ttu-id="a169e-379">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a169e-379">Click **Create**.</span></span>
3. <span data-ttu-id="a169e-380">Po dokončení vytvoření se zobrazí **Data Factory** okno, jak je znázorněno na následujícím obrázku: ![Domovská stránka objektu pro vytváření dat](./media/data-factory-azure-blob-connector/data-factory-home-page.png)</span><span class="sxs-lookup"><span data-stu-id="a169e-380">After the creation is complete, you see the **Data Factory** blade as shown in the following image: ![Data factory home page](./media/data-factory-azure-blob-connector/data-factory-home-page.png)</span></span>

### <a name="copy-wizard"></a><span data-ttu-id="a169e-381">Průvodce kopírováním</span><span class="sxs-lookup"><span data-stu-id="a169e-381">Copy Wizard</span></span>
1. <span data-ttu-id="a169e-382">Na domovské stránce objektu pro vytváření dat klikněte **kopírování dat [PREVIEW]** dlaždici spustíte **Průvodce kopírováním dat** na samostatné kartě.</span><span class="sxs-lookup"><span data-stu-id="a169e-382">On the Data Factory home page, click the **Copy data [PREVIEW]** tile to launch **Copy Data Wizard** in a separate tab.</span></span>    
    
    > [!NOTE]
    >    <span data-ttu-id="a169e-383">Pokud se zobrazí, že webový prohlížeč zasekl ve fázi "autorizace …", zakažte/zrušte zaškrtnutí políčka **blokovat soubory cookie třetích stran a data lokality** nastavení (nebo) zachovat povolené a vytvořte výjimku pro **login.microsoftonline.com** a poté se pokuste spustit průvodce znovu.</span><span class="sxs-lookup"><span data-stu-id="a169e-383">If you see that the web browser is stuck at "Authorizing...", disable/uncheck **Block third-party cookies and site data** setting (or) keep it enabled and create an exception for **login.microsoftonline.com** and then try launching the wizard again.</span></span>
2. <span data-ttu-id="a169e-384">Na stránce **Vlastnosti**:</span><span class="sxs-lookup"><span data-stu-id="a169e-384">In the **Properties** page:</span></span>
    1. <span data-ttu-id="a169e-385">Zadejte **CopyPipeline** pro **název úlohy**.</span><span class="sxs-lookup"><span data-stu-id="a169e-385">Enter **CopyPipeline** for **Task name**.</span></span> <span data-ttu-id="a169e-386">Název úlohy je název kanálu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="a169e-386">The task name is the name of the pipeline in your data factory.</span></span>
    2. <span data-ttu-id="a169e-387">Zadejte **popis** pro úlohu (volitelné).</span><span class="sxs-lookup"><span data-stu-id="a169e-387">Enter a **description** for the task (optional).</span></span>
    3. <span data-ttu-id="a169e-388">Pro **cadence úloh nebo plán úloh**, zachovat **spuštění pravidelně podle plánu** možnost.</span><span class="sxs-lookup"><span data-stu-id="a169e-388">For **Task cadence or Task schedule**, keep the **Run regularly on schedule** option.</span></span> <span data-ttu-id="a169e-389">Pokud chcete spustit tuto úlohu jenom jednou místo spustit opakovaně podle plánu, vyberte **spustit jednou nyní**.</span><span class="sxs-lookup"><span data-stu-id="a169e-389">If you want to run this task only once instead of run repeatedly on a schedule, select **Run once now**.</span></span> <span data-ttu-id="a169e-390">Pokud vyberete, **spustit jednou nyní** možnost, [jednorázového kanálu](data-factory-create-pipelines.md#onetime-pipeline) je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="a169e-390">If you select, **Run once now** option, a [one-time pipeline](data-factory-create-pipelines.md#onetime-pipeline) is created.</span></span> 
    4. <span data-ttu-id="a169e-391">Potvrďte nastavení pro **opakovaná vzor**.</span><span class="sxs-lookup"><span data-stu-id="a169e-391">Keep the settings for **Recurring pattern**.</span></span> <span data-ttu-id="a169e-392">Tato úloha se spustí každý den mezi počáteční a koncový čas, které zadáte v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="a169e-392">This task runs daily between the start and end times you specify in the next step.</span></span>
    5. <span data-ttu-id="a169e-393">Změna **datum a čas zahájení** k **21/04/2017**.</span><span class="sxs-lookup"><span data-stu-id="a169e-393">Change the **Start date time** to **04/21/2017**.</span></span> 
    6. <span data-ttu-id="a169e-394">Změna **datum a čas ukončení** k **04/25/2017**.</span><span class="sxs-lookup"><span data-stu-id="a169e-394">Change the **End date time** to **04/25/2017**.</span></span> <span data-ttu-id="a169e-395">Můžete zadat datum místo projdete kalendáři.</span><span class="sxs-lookup"><span data-stu-id="a169e-395">You may want to type the date instead of browsing through the calendar.</span></span>     
    8. <span data-ttu-id="a169e-396">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="a169e-396">Click **Next**.</span></span>
      <span data-ttu-id="a169e-397">![Nástroj pro kopírování – stránka vlastností](./media/data-factory-azure-blob-connector/copy-tool-properties-page.png)</span><span class="sxs-lookup"><span data-stu-id="a169e-397">![Copy Tool - Properties page](./media/data-factory-azure-blob-connector/copy-tool-properties-page.png)</span></span> 
3. <span data-ttu-id="a169e-398">Na stránce **Source data store** (Zdrojové úložiště dat) klikněte na dlaždici **Azure Blob Storage**.</span><span class="sxs-lookup"><span data-stu-id="a169e-398">On the **Source data store** page, click **Azure Blob Storage** tile.</span></span> <span data-ttu-id="a169e-399">Tato stránka slouží k zadání zdrojového úložiště dat pro úlohu kopírování.</span><span class="sxs-lookup"><span data-stu-id="a169e-399">You use this page to specify the source data store for the copy task.</span></span> <span data-ttu-id="a169e-400">Můžete použít existující propojenou službu úložiště dat nebo zadat nové úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="a169e-400">You can use an existing data store linked service (or) specify a new data store.</span></span> <span data-ttu-id="a169e-401">Pokud chcete použít existující propojenou službu, vyberte **z existujících PROPOJENÝCH služeb** a vyberte požadovanou propojenou službu.</span><span class="sxs-lookup"><span data-stu-id="a169e-401">To use an existing linked service, you would select **FROM EXISTING LINKED SERVICES** and select the right linked service.</span></span> 
    <span data-ttu-id="a169e-402">![Nástroj pro kopírování – stránka zdrojového úložiště dat](./media/data-factory-azure-blob-connector/copy-tool-source-data-store-page.png)</span><span class="sxs-lookup"><span data-stu-id="a169e-402">![Copy Tool - Source data store page](./media/data-factory-azure-blob-connector/copy-tool-source-data-store-page.png)</span></span>
4. <span data-ttu-id="a169e-403">Na stránce **Specify the Azure Blob storage account** (Zadejte účet Azure Blob Storage):</span><span class="sxs-lookup"><span data-stu-id="a169e-403">On the **Specify the Azure Blob storage account** page:</span></span>
   1. <span data-ttu-id="a169e-404">Zachovat automaticky generovaný název pro **název připojení**.</span><span class="sxs-lookup"><span data-stu-id="a169e-404">Keep the auto-generated name for **Connection name**.</span></span> <span data-ttu-id="a169e-405">Název připojení je název propojené služby typu: Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="a169e-405">The connection name is the name of the linked service of type: Azure Storage.</span></span> 
   2. <span data-ttu-id="a169e-406">Ujistěte se, že je pro položku **Metoda výběru účtu** vybrána možnost **Z předplatných Azure**.</span><span class="sxs-lookup"><span data-stu-id="a169e-406">Confirm that **From Azure subscriptions** option is selected for **Account selection method**.</span></span>
   3. <span data-ttu-id="a169e-407">Vyberte předplatné Azure nebo ponechte **Vybrat vše** pro **předplatné**.</span><span class="sxs-lookup"><span data-stu-id="a169e-407">Select your Azure subscription or keep **Select all** for **Azure subscription**.</span></span>   
   4. <span data-ttu-id="a169e-408">V seznamu účtů úložiště Azure dostupných ve zvoleném předplatném vyberte požadovaný **účet úložiště Azure**.</span><span class="sxs-lookup"><span data-stu-id="a169e-408">Select an **Azure storage account** from the list of Azure storage accounts available in the selected subscription.</span></span> <span data-ttu-id="a169e-409">Můžete také zadat nastavení účtu úložiště ručně tak, že vyberete **zadat ručně** možnost **účet metodu výběru**.</span><span class="sxs-lookup"><span data-stu-id="a169e-409">You can also choose to enter storage account settings manually by selecting **Enter manually** option for the **Account selection method**.</span></span>
   5. <span data-ttu-id="a169e-410">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="a169e-410">Click **Next**.</span></span> 
      <span data-ttu-id="a169e-411">![Nástroj pro kopírování – zadání účtu Azure Blob storage](./media/data-factory-azure-blob-connector/copy-tool-specify-azure-blob-storage-account.png)</span><span class="sxs-lookup"><span data-stu-id="a169e-411">![Copy Tool - Specify the Azure Blob storage account](./media/data-factory-azure-blob-connector/copy-tool-specify-azure-blob-storage-account.png)</span></span>
5. <span data-ttu-id="a169e-412">Na stránce **Choose the input file or folder** (Zvolte vstupní soubor nebo složku):</span><span class="sxs-lookup"><span data-stu-id="a169e-412">On **Choose the input file or folder** page:</span></span>
   1. <span data-ttu-id="a169e-413">Klikněte dvakrát na **adfblobcontainer**.</span><span class="sxs-lookup"><span data-stu-id="a169e-413">Double-click **adfblobcontainer**.</span></span>
   2. <span data-ttu-id="a169e-414">Vyberte **vstupní**a klikněte na tlačítko **zvolte**.</span><span class="sxs-lookup"><span data-stu-id="a169e-414">Select **input**, and click **Choose**.</span></span> <span data-ttu-id="a169e-415">V tomto návodu vyberte vstupní složky.</span><span class="sxs-lookup"><span data-stu-id="a169e-415">In this walkthrough, you select the input folder.</span></span> <span data-ttu-id="a169e-416">Můžete třeba také vybrat soubor emp.txt ve složce místo.</span><span class="sxs-lookup"><span data-stu-id="a169e-416">You could also select the emp.txt file in the folder instead.</span></span> 
      <span data-ttu-id="a169e-417">![Nástroj pro kopírování – volba vstupního souboru nebo složky](./media/data-factory-azure-blob-connector/copy-tool-choose-input-file-or-folder.png)</span><span class="sxs-lookup"><span data-stu-id="a169e-417">![Copy Tool - Choose the input file or folder](./media/data-factory-azure-blob-connector/copy-tool-choose-input-file-or-folder.png)</span></span>
6. <span data-ttu-id="a169e-418">Na **zvolte vstupní soubor nebo složku** stránky:</span><span class="sxs-lookup"><span data-stu-id="a169e-418">On the **Choose the input file or folder** page:</span></span>
    1. <span data-ttu-id="a169e-419">Potvrďte, že **souboru nebo složky** je nastaven na **adfblobconnector/vstup**.</span><span class="sxs-lookup"><span data-stu-id="a169e-419">Confirm that the **file or folder** is set to **adfblobconnector/input**.</span></span> <span data-ttu-id="a169e-420">Pokud jsou soubory do podsložek, například 2017/04/01, 2017/04/02 a tak dále, zadání adfblobconnector / / {year} / {month} / {day} pro soubor nebo složku.</span><span class="sxs-lookup"><span data-stu-id="a169e-420">If the files are in sub folders, for example, 2017/04/01, 2017/04/02, and so on, enter adfblobconnector/input/{year}/{month}/{day} for file or folder.</span></span> <span data-ttu-id="a169e-421">Po stisknutí klávesy TAB mimo textového pole, zobrazí se tří rozevíracích seznamech vyberte formáty (rrrr) rok, měsíc (MM) a den (dd).</span><span class="sxs-lookup"><span data-stu-id="a169e-421">When you press TAB out of the text box, you see three drop-down lists to select formats for year (yyyy), month (MM), and day (dd).</span></span> 
    2. <span data-ttu-id="a169e-422">Nenastavujte **zkopírujte soubor rekurzivně**.</span><span class="sxs-lookup"><span data-stu-id="a169e-422">Do not set **Copy file recursively**.</span></span> <span data-ttu-id="a169e-423">Tuto možnost vyberte k rekurzivnímu křížovou prostřednictvím složek souborů ke zkopírování do cílového umístění.</span><span class="sxs-lookup"><span data-stu-id="a169e-423">Select this option to recursively traverse through folders for files to be copied to the destination.</span></span> 
    3. <span data-ttu-id="a169e-424">Nechcete **binární kopie** možnost.</span><span class="sxs-lookup"><span data-stu-id="a169e-424">Do not the **binary copy** option.</span></span> <span data-ttu-id="a169e-425">Vyberte tuto možnost, chcete-li provést binární kopii zdrojového souboru do cílového umístění.</span><span class="sxs-lookup"><span data-stu-id="a169e-425">Select this option to perform a binary copy of source file to the destination.</span></span> <span data-ttu-id="a169e-426">Nevybírejte v tomto návodu tak, aby se zobrazí další možnosti v dalších stránkách.</span><span class="sxs-lookup"><span data-stu-id="a169e-426">Do not select for this walkthrough so that you can see more options in the next pages.</span></span> 
    4. <span data-ttu-id="a169e-427">Potvrďte, že **typ komprese** je nastaven na **žádné**.</span><span class="sxs-lookup"><span data-stu-id="a169e-427">Confirm that the **Compression type** is set to **None**.</span></span> <span data-ttu-id="a169e-428">Hodnotu pro tuto možnost vyberte, pokud jsou v některém z podporovaných formátů komprimované zdrojové soubory.</span><span class="sxs-lookup"><span data-stu-id="a169e-428">Select a value for this option if your source files are compressed in one of the supported formats.</span></span> 
    5. <span data-ttu-id="a169e-429">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="a169e-429">Click **Next**.</span></span>
    <span data-ttu-id="a169e-430">![Nástroj pro kopírování – volba vstupního souboru nebo složky](./media/data-factory-azure-blob-connector/chose-input-file-folder.png)</span><span class="sxs-lookup"><span data-stu-id="a169e-430">![Copy Tool - Choose the input file or folder](./media/data-factory-azure-blob-connector/chose-input-file-folder.png)</span></span> 
7. <span data-ttu-id="a169e-431">Na stránce **Nastavení formátu souboru** jsou uvedeny oddělovače a schéma, které je automaticky zjištěno průvodcem při analýze souboru.</span><span class="sxs-lookup"><span data-stu-id="a169e-431">On the **File format settings** page, you see the delimiters and the schema that is auto-detected by the wizard by parsing the file.</span></span> 
    1. <span data-ttu-id="a169e-432">Potvrďte následující možnosti:.</span><span class="sxs-lookup"><span data-stu-id="a169e-432">Confirm the following options: a.</span></span> <span data-ttu-id="a169e-433">**Formát souboru** je nastaven na **formátu textu**.</span><span class="sxs-lookup"><span data-stu-id="a169e-433">The **file format** is set to **Text format**.</span></span> <span data-ttu-id="a169e-434">Můžete zobrazit všechny podporované formáty v rozevíracím seznamu.</span><span class="sxs-lookup"><span data-stu-id="a169e-434">You can see all the supported formats in the drop-down list.</span></span> <span data-ttu-id="a169e-435">Příklad: formát JSON, Avro, ORC, Parquet.</span><span class="sxs-lookup"><span data-stu-id="a169e-435">For example: JSON, Avro, ORC, Parquet.</span></span>
        <span data-ttu-id="a169e-436">b.</span><span class="sxs-lookup"><span data-stu-id="a169e-436">b.</span></span> <span data-ttu-id="a169e-437">**Sloupec oddělovač** je nastaven na `Comma (,)`.</span><span class="sxs-lookup"><span data-stu-id="a169e-437">The **column delimiter** is set to `Comma (,)`.</span></span> <span data-ttu-id="a169e-438">Můžete zobrazit další sloupec oddělovače podporovaných službou Data Factory v rozevíracím seznamu.</span><span class="sxs-lookup"><span data-stu-id="a169e-438">You can see the other column delimiters supported by Data Factory in the drop-down list.</span></span> <span data-ttu-id="a169e-439">Můžete také zadat vlastní oddělovač.</span><span class="sxs-lookup"><span data-stu-id="a169e-439">You can also specify a custom delimiter.</span></span>
        <span data-ttu-id="a169e-440">c.</span><span class="sxs-lookup"><span data-stu-id="a169e-440">c.</span></span> <span data-ttu-id="a169e-441">**Oddělovač řádků** je nastaven na `Carriage Return + Line feed (\r\n)`.</span><span class="sxs-lookup"><span data-stu-id="a169e-441">The **row delimiter** is set to `Carriage Return + Line feed (\r\n)`.</span></span> <span data-ttu-id="a169e-442">Můžete zobrazit další řádek oddělovače podporovaných službou Data Factory v rozevíracím seznamu.</span><span class="sxs-lookup"><span data-stu-id="a169e-442">You can see the other row delimiters supported by Data Factory in the drop-down list.</span></span> <span data-ttu-id="a169e-443">Můžete také zadat vlastní oddělovač.</span><span class="sxs-lookup"><span data-stu-id="a169e-443">You can also specify a custom delimiter.</span></span>
        <span data-ttu-id="a169e-444">d.</span><span class="sxs-lookup"><span data-stu-id="a169e-444">d.</span></span> <span data-ttu-id="a169e-445">**Přeskočit počet řádků** je nastaven na **0**.</span><span class="sxs-lookup"><span data-stu-id="a169e-445">The **skip line count** is set to **0**.</span></span> <span data-ttu-id="a169e-446">Pokud chcete po zadání několika řádků lze vynechat v horní části souboru, zadejte zde číslo.</span><span class="sxs-lookup"><span data-stu-id="a169e-446">If you want a few lines to be skipped at the top of the file, enter the number here.</span></span>
        <span data-ttu-id="a169e-447">e.</span><span class="sxs-lookup"><span data-stu-id="a169e-447">e.</span></span>  <span data-ttu-id="a169e-448">**První řádek dat obsahuje názvy sloupců** není nastaven.</span><span class="sxs-lookup"><span data-stu-id="a169e-448">The **first data row contains column names** is not set.</span></span> <span data-ttu-id="a169e-449">Pokud zdrojové soubory obsahují názvy sloupců v prvním řádku, vyberte tuto možnost.</span><span class="sxs-lookup"><span data-stu-id="a169e-449">If the source files contain column names in the first row, select this option.</span></span>
        <span data-ttu-id="a169e-450">f.</span><span class="sxs-lookup"><span data-stu-id="a169e-450">f.</span></span> <span data-ttu-id="a169e-451">**Považovat prázdný sloupec hodnota null** je možnost nastavena.</span><span class="sxs-lookup"><span data-stu-id="a169e-451">The **treat empty column value as null** option is set.</span></span>
    2. <span data-ttu-id="a169e-452">Rozbalte položku **upřesňující nastavení** zobrazíte upřesňující možnost k dispozici.</span><span class="sxs-lookup"><span data-stu-id="a169e-452">Expand **Advanced settings** to see advanced option available.</span></span>
    3. <span data-ttu-id="a169e-453">V dolní části stránky, najdete v článku **preview** dat ze souboru emp.txt.</span><span class="sxs-lookup"><span data-stu-id="a169e-453">At the bottom of the page, see the **preview** of data from the emp.txt file.</span></span>
    4. <span data-ttu-id="a169e-454">Klikněte na tlačítko **schématu** karta v dolní části zobrazíte schéma, které Průvodce kopírováním vyvozena na základě dat ve zdrojovém souboru.</span><span class="sxs-lookup"><span data-stu-id="a169e-454">Click **SCHEMA** tab at the bottom to see the schema that the copy wizard inferred by looking at the data in the source file.</span></span>
    5. <span data-ttu-id="a169e-455">Po zkontrolování oddělovačů a náhledu dat klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="a169e-455">Click **Next** after you review the delimiters and preview data.</span></span>
    <span data-ttu-id="a169e-456">![Nástroj pro kopírování – nastavení formátu souboru](./media/data-factory-azure-blob-connector/copy-tool-file-format-settings.png)</span><span class="sxs-lookup"><span data-stu-id="a169e-456">![Copy Tool - File format settings](./media/data-factory-azure-blob-connector/copy-tool-file-format-settings.png)</span></span>  
8. <span data-ttu-id="a169e-457">Na **úložiště dat cílové stránky**, vyberte **Azure Blob Storage**a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="a169e-457">On the **Destination data store page**, select **Azure Blob Storage**, and click **Next**.</span></span> <span data-ttu-id="a169e-458">Úložiště objektů Blob Azure používají jako obou zdrojové a cílové úložištích dat v tomto návodu.</span><span class="sxs-lookup"><span data-stu-id="a169e-458">You are using the Azure Blob Storage as both the source and destination data stores in this walkthrough.</span></span>    
    <span data-ttu-id="a169e-459">![Nástroj pro kopírování – vyberte cílového úložiště dat](media/data-factory-azure-blob-connector/select-destination-data-store.png)</span><span class="sxs-lookup"><span data-stu-id="a169e-459">![Copy Tool - select destination data store](media/data-factory-azure-blob-connector/select-destination-data-store.png)</span></span>
9. <span data-ttu-id="a169e-460">Na **zadejte účet úložiště Azure Blob** stránky:</span><span class="sxs-lookup"><span data-stu-id="a169e-460">On **Specify the Azure Blob storage account** page:</span></span>
   1. <span data-ttu-id="a169e-461">Zadejte **AzureStorageLinkedService** pro **název připojení** pole.</span><span class="sxs-lookup"><span data-stu-id="a169e-461">Enter **AzureStorageLinkedService** for the **Connection name** field.</span></span>
   2. <span data-ttu-id="a169e-462">Ujistěte se, že je pro položku **Metoda výběru účtu** vybrána možnost **Z předplatných Azure**.</span><span class="sxs-lookup"><span data-stu-id="a169e-462">Confirm that **From Azure subscriptions** option is selected for **Account selection method**.</span></span>
   3. <span data-ttu-id="a169e-463">Vyberte své **předplatné** Azure.</span><span class="sxs-lookup"><span data-stu-id="a169e-463">Select your Azure **subscription**.</span></span>  
   4. <span data-ttu-id="a169e-464">Vyberte účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="a169e-464">Select your Azure storage account.</span></span> 
   5. <span data-ttu-id="a169e-465">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="a169e-465">Click **Next**.</span></span>     
10. <span data-ttu-id="a169e-466">Na **zvolit výstupní soubor nebo složku** stránky:</span><span class="sxs-lookup"><span data-stu-id="a169e-466">On the **Choose the output file or folder** page:</span></span> 
    6. <span data-ttu-id="a169e-467">Zadejte **cesta ke složce** jako **adfblobconnector výstupní / {year} / {month} / {day}**.</span><span class="sxs-lookup"><span data-stu-id="a169e-467">specify **Folder path** as **adfblobconnector/output/{year}/{month}/{day}**.</span></span> <span data-ttu-id="a169e-468">Zadejte **KARTĚ**.</span><span class="sxs-lookup"><span data-stu-id="a169e-468">Enter **TAB**.</span></span>
    7. <span data-ttu-id="a169e-469">Pro **roku**, vyberte **rrrr**.</span><span class="sxs-lookup"><span data-stu-id="a169e-469">For the **year**, select **yyyy**.</span></span>
    8. <span data-ttu-id="a169e-470">Pro **měsíc**, potvrďte, že je nastavena na **MM**.</span><span class="sxs-lookup"><span data-stu-id="a169e-470">For the **month**, confirm that it is set to **MM**.</span></span>
    9. <span data-ttu-id="a169e-471">Pro **den**, potvrďte, že je nastavena na **dd**.</span><span class="sxs-lookup"><span data-stu-id="a169e-471">For the **day**, confirm that it is set to **dd**.</span></span>
    10. <span data-ttu-id="a169e-472">Potvrďte, že **typ komprese** je nastaven na **žádné**.</span><span class="sxs-lookup"><span data-stu-id="a169e-472">Confirm that the **compression type** is set to **None**.</span></span>
    11. <span data-ttu-id="a169e-473">Potvrďte, že **zkopírujte chování** je nastaven na **sloučení souborů**.</span><span class="sxs-lookup"><span data-stu-id="a169e-473">Confirm that the **copy behavior** is set to **Merge files**.</span></span> <span data-ttu-id="a169e-474">Pokud výstupní soubor s tímto názvem již existuje, je přidán nový obsah do stejného souboru na konci.</span><span class="sxs-lookup"><span data-stu-id="a169e-474">If the output file with the same name already exists, the new content is added to the same file at the end.</span></span>
    12. <span data-ttu-id="a169e-475">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="a169e-475">Click **Next**.</span></span>
    <span data-ttu-id="a169e-476">![Nástroj pro kopírování – volba výstupního souboru nebo složky](media/data-factory-azure-blob-connector/choose-the-output-file-or-folder.png)</span><span class="sxs-lookup"><span data-stu-id="a169e-476">![Copy Tool - Choose output file or folder](media/data-factory-azure-blob-connector/choose-the-output-file-or-folder.png)</span></span>
11. <span data-ttu-id="a169e-477">Na **nastavení formátu souboru** stránka, zkontrolujte nastavení a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="a169e-477">On the **File format settings** page, review the settings, and click **Next**.</span></span> <span data-ttu-id="a169e-478">Jedním z dalších možností je přidat hlavičku do výstupního souboru.</span><span class="sxs-lookup"><span data-stu-id="a169e-478">One of the additional options here is to add a header to the output file.</span></span> <span data-ttu-id="a169e-479">Pokud tuto možnost vyberete, se přidá řádek záhlaví s názvy sloupců ze schématu zdroje.</span><span class="sxs-lookup"><span data-stu-id="a169e-479">If you select that option, a header row is added with names of the columns from the schema of the source.</span></span> <span data-ttu-id="a169e-480">Při zobrazení schématu zdroje, můžete přejmenovat výchozí názvy sloupců.</span><span class="sxs-lookup"><span data-stu-id="a169e-480">You can rename the default column names when viewing the schema for the source.</span></span> <span data-ttu-id="a169e-481">První sloupec můžete například změnit křestní jméno a příjmení druhý sloupec.</span><span class="sxs-lookup"><span data-stu-id="a169e-481">For example, you could change the first column to First Name and second column to Last Name.</span></span> <span data-ttu-id="a169e-482">Potom výstupní soubor je vytvořen s záhlaví s těmito názvy jako názvy sloupců.</span><span class="sxs-lookup"><span data-stu-id="a169e-482">Then, the output file is generated with a header with these names as column names.</span></span> 
    <span data-ttu-id="a169e-483">![Nástroj pro kopírování – nastavení formátu souboru pro cíl](media/data-factory-azure-blob-connector/file-format-destination.png)</span><span class="sxs-lookup"><span data-stu-id="a169e-483">![Copy Tool - File format settings for destination](media/data-factory-azure-blob-connector/file-format-destination.png)</span></span>
12. <span data-ttu-id="a169e-484">Na **nastavení výkonu** potvrďte, že **cloudu jednotky** a **paralelní kopie** jsou nastaveny na **automaticky**a klikněte na tlačítko Další.</span><span class="sxs-lookup"><span data-stu-id="a169e-484">On the **Performance settings** page, confirm that **cloud units** and **parallel copies** are set to **Auto**, and click Next.</span></span> <span data-ttu-id="a169e-485">Podrobnosti o těchto nastaveních najdete v tématu [zkopírujte aktivity výkonu a vyladění průvodce](data-factory-copy-activity-performance.md#parallel-copy).</span><span class="sxs-lookup"><span data-stu-id="a169e-485">For details about these settings, see [Copy activity performance and tuning guide](data-factory-copy-activity-performance.md#parallel-copy).</span></span>
    <span data-ttu-id="a169e-486">![Nástroj pro kopírování – nastavení výkonu](media/data-factory-azure-blob-connector/copy-performance-settings.png)</span><span class="sxs-lookup"><span data-stu-id="a169e-486">![Copy Tool - Performance settings](media/data-factory-azure-blob-connector/copy-performance-settings.png)</span></span> 
14. <span data-ttu-id="a169e-487">Na **Souhrn** zkontrolujte všechna nastavení (Vlastnosti úlohy, nastavení pro zdrojové a cílové a Kopírovat nastavení) a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="a169e-487">On the **Summary** page, review all settings (task properties, settings for source and destination, and copy settings), and click **Next**.</span></span>
    <span data-ttu-id="a169e-488">![Nástroj pro kopírování – stránka souhrnu](media/data-factory-azure-blob-connector/copy-tool-summary-page.png)</span><span class="sxs-lookup"><span data-stu-id="a169e-488">![Copy Tool - Summary page](media/data-factory-azure-blob-connector/copy-tool-summary-page.png)</span></span>
15. <span data-ttu-id="a169e-489">Na stránce **Souhrn** zkontrolujte informace a klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="a169e-489">Review information in the **Summary** page, and click **Finish**.</span></span> <span data-ttu-id="a169e-490">Průvodce v objektu pro vytváření dat (ze kterého jste průvodce kopírováním spustili) vytvoří dvě propojené služby, dvě datové sady (vstupní a výstupní) a jeden kanál.</span><span class="sxs-lookup"><span data-stu-id="a169e-490">The wizard creates two linked services, two datasets (input and output), and one pipeline in the data factory (from where you launched the Copy Wizard).</span></span>
    <span data-ttu-id="a169e-491">![Nástroj pro kopírování – stránka nasazení](media/data-factory-azure-blob-connector/copy-tool-deployment-page.png)</span><span class="sxs-lookup"><span data-stu-id="a169e-491">![Copy Tool - Deployment page](media/data-factory-azure-blob-connector/copy-tool-deployment-page.png)</span></span>

### <a name="monitor-the-pipeline-copy-task"></a><span data-ttu-id="a169e-492">Monitorování kanálu (úlohy kopie)</span><span class="sxs-lookup"><span data-stu-id="a169e-492">Monitor the pipeline (copy task)</span></span>

1. <span data-ttu-id="a169e-493">Klikněte na odkaz `Click here to monitor copy pipeline` na **nasazení** stránky.</span><span class="sxs-lookup"><span data-stu-id="a169e-493">Click the link `Click here to monitor copy pipeline` on the **Deployment** page.</span></span> 
2. <span data-ttu-id="a169e-494">Měli byste vidět **sledovat a spravovat aplikace** na samostatné kartě.  ![Sledování a správě aplikací](media/data-factory-azure-blob-connector/monitor-manage-app.png)</span><span class="sxs-lookup"><span data-stu-id="a169e-494">You should see the **Monitor and Manage application** in a separate tab.  ![Monitor and Manage App](media/data-factory-azure-blob-connector/monitor-manage-app.png)</span></span>
3. <span data-ttu-id="a169e-495">Změna **spustit** čas od nejvyšší `04/19/2017` a **end** čas k `04/27/2017`a potom klikněte na **použít**.</span><span class="sxs-lookup"><span data-stu-id="a169e-495">Change the **start** time at the top to `04/19/2017` and **end** time to `04/27/2017`, and then click **Apply**.</span></span> 
4. <span data-ttu-id="a169e-496">Měli byste vidět pět oken aktivity v **aktivity WINDOWS** seznamu.</span><span class="sxs-lookup"><span data-stu-id="a169e-496">You should see five activity windows in the **ACTIVITY WINDOWS** list.</span></span> <span data-ttu-id="a169e-497">**WindowStart** časy by mělo zahrnovat všechny dny od začátku kanálu do kanálu koncový čas.</span><span class="sxs-lookup"><span data-stu-id="a169e-497">The **WindowStart** times should cover all days from pipeline start to pipeline end times.</span></span> 
5. <span data-ttu-id="a169e-498">Klikněte na tlačítko **aktualizovat** tlačítko pro **aktivity WINDOWS** seznamu několikrát, dokud se zobrazí stav všech aktivity windows je nastaven na hodnotu Připraveno.</span><span class="sxs-lookup"><span data-stu-id="a169e-498">Click **Refresh** button for the **ACTIVITY WINDOWS** list a few times until you see the status of all the activity windows is set to Ready.</span></span> 
6. <span data-ttu-id="a169e-499">Nyní ověřte, že jsou generovány výstupní soubory v zadané výstupní složce adfblobconnector kontejneru.</span><span class="sxs-lookup"><span data-stu-id="a169e-499">Now, verify that the output files are generated in the output folder of adfblobconnector container.</span></span> <span data-ttu-id="a169e-500">Měli byste vidět následující strukturu složek do výstupní složky:</span><span class="sxs-lookup"><span data-stu-id="a169e-500">You should see the following folder structure in the output folder:</span></span> 
    ```
    2017/04/21
    2017/04/22
    2017/04/23
    2017/04/24
    2017/04/25    
    ```
<span data-ttu-id="a169e-501">Podrobné informace o monitorování a Správa objektů pro vytváření dat najdete v tématu [monitorování a Správa kanálů služby Data Factory](data-factory-monitor-manage-app.md) článku.</span><span class="sxs-lookup"><span data-stu-id="a169e-501">For detailed information about monitoring and managing data factories, see [Monitor and manage Data Factory pipeline](data-factory-monitor-manage-app.md) article.</span></span> 
 
### <a name="data-factory-entities"></a><span data-ttu-id="a169e-502">Entity objektu pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="a169e-502">Data Factory entities</span></span>
<span data-ttu-id="a169e-503">Nyní přejděte zpět na kartě s domovské stránce objektu pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="a169e-503">Now, switch back to the tab with the Data Factory home page.</span></span> <span data-ttu-id="a169e-504">Všimněte si, že existují dvě propojené služby, dvě datové sady a jeden kanál v datové továrně teď.</span><span class="sxs-lookup"><span data-stu-id="a169e-504">Notice that there are two linked services, two datasets, and one pipeline in your data factory now.</span></span> 

![Domovská stránka objektu pro vytváření dat s entitami](media/data-factory-azure-blob-connector/data-factory-home-page-with-numbers.png)

<span data-ttu-id="a169e-506">Klikněte na tlačítko **vytvořit a nasadit** ke spuštění editoru služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="a169e-506">Click **Author and deploy** to launch Data Factory Editor.</span></span> 

![Data Factory Editor](media/data-factory-azure-blob-connector/data-factory-editor.png)

<span data-ttu-id="a169e-508">V datové továrně byste měli vidět následující entity služby Data Factory:</span><span class="sxs-lookup"><span data-stu-id="a169e-508">You should see the following Data Factory entities in your data factory:</span></span> 

 - <span data-ttu-id="a169e-509">Dvě propojené služby.</span><span class="sxs-lookup"><span data-stu-id="a169e-509">Two linked services.</span></span> <span data-ttu-id="a169e-510">Jeden pro zdroji a ostatní jeden pro cíl.</span><span class="sxs-lookup"><span data-stu-id="a169e-510">One for the source and the other one for the destination.</span></span> <span data-ttu-id="a169e-511">Propojené služby odkazovat na stejný účet služby Azure Storage v tomto návodu.</span><span class="sxs-lookup"><span data-stu-id="a169e-511">Both the linked services refer to the same Azure Storage account in this walkthrough.</span></span> 
 - <span data-ttu-id="a169e-512">Dvě datové sady.</span><span class="sxs-lookup"><span data-stu-id="a169e-512">Two datasets.</span></span> <span data-ttu-id="a169e-513">Vstupní datové sady a datovou sadu výstupů.</span><span class="sxs-lookup"><span data-stu-id="a169e-513">An input dataset and an output dataset.</span></span> <span data-ttu-id="a169e-514">V tomto návodu obě používat stejný kontejner objektů blob ale odkazovat na jiné složky (vstup a výstup).</span><span class="sxs-lookup"><span data-stu-id="a169e-514">In this walkthrough, both use the same blob container but refer to different folders (input and output).</span></span>
 - <span data-ttu-id="a169e-515">Kanál.</span><span class="sxs-lookup"><span data-stu-id="a169e-515">A pipeline.</span></span> <span data-ttu-id="a169e-516">Kanál obsahuje kopii aktivit, které používají zdroj objektů blob a podřízený objekt blob ke zkopírování dat z umístění objektu blob Azure do jiného umístění objektu blob Azure.</span><span class="sxs-lookup"><span data-stu-id="a169e-516">The pipeline contains a copy activity that uses a blob source and a blob sink to copy data from an Azure blob location to another Azure blob location.</span></span> 

<span data-ttu-id="a169e-517">Následující části obsahují další informace o těchto entitách.</span><span class="sxs-lookup"><span data-stu-id="a169e-517">The following sections provide more information about these entities.</span></span> 

#### <a name="linked-services"></a><span data-ttu-id="a169e-518">Propojené služby</span><span class="sxs-lookup"><span data-stu-id="a169e-518">Linked services</span></span>
<span data-ttu-id="a169e-519">Měli byste vidět dvě propojené služby.</span><span class="sxs-lookup"><span data-stu-id="a169e-519">You should see two linked services.</span></span> <span data-ttu-id="a169e-520">Jeden pro zdroji a ostatní jeden pro cíl.</span><span class="sxs-lookup"><span data-stu-id="a169e-520">One for the source and the other one for the destination.</span></span> <span data-ttu-id="a169e-521">V tomto návodu podívejte se i definice stejné s výjimkou názvy.</span><span class="sxs-lookup"><span data-stu-id="a169e-521">In this walkthrough, both definitions look the same except for the names.</span></span> <span data-ttu-id="a169e-522">**Typ** propojené služby je nastaven na **azurestorage**.</span><span class="sxs-lookup"><span data-stu-id="a169e-522">The **type** of the linked service is set to **AzureStorage**.</span></span> <span data-ttu-id="a169e-523">Nejdůležitější vlastnost definice propojené služby **connectionString**, který je využíván jiným objekt pro vytváření dat pro připojení k účtu úložiště Azure za běhu.</span><span class="sxs-lookup"><span data-stu-id="a169e-523">Most important property of the linked service definition is the **connectionString**, which is used by Data Factory to connect to your Azure Storage account at runtime.</span></span> <span data-ttu-id="a169e-524">Ignorujte hubName vlastnost v definici.</span><span class="sxs-lookup"><span data-stu-id="a169e-524">Ignore the hubName property in the definition.</span></span> 

##### <a name="source-blob-storage-linked-service"></a><span data-ttu-id="a169e-525">Zdrojový objekt blob propojená služba úložiště</span><span class="sxs-lookup"><span data-stu-id="a169e-525">Source blob storage linked service</span></span>
```json
{
    "name": "Source-BlobStorage-z4y",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=mystorageaccount;AccountKey=**********"
        }
    }
}
```

##### <a name="destination-blob-storage-linked-service"></a><span data-ttu-id="a169e-526">Cílový objekt blob propojená služba úložiště</span><span class="sxs-lookup"><span data-stu-id="a169e-526">Destination blob storage linked service</span></span>

```json
{
    "name": "Destination-BlobStorage-z4y",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=mystorageaccount;AccountKey=**********"
        }
    }
}
```

<span data-ttu-id="a169e-527">Další informace o propojené služby Azure Storage najdete v tématu [propojené vlastnosti služby](#linked-service-properties) části.</span><span class="sxs-lookup"><span data-stu-id="a169e-527">For more information about Azure Storage linked service, see [Linked service properties](#linked-service-properties) section.</span></span> 

#### <a name="datasets"></a><span data-ttu-id="a169e-528">Datové sady</span><span class="sxs-lookup"><span data-stu-id="a169e-528">Datasets</span></span>
<span data-ttu-id="a169e-529">Existují dvě datové sady: vstupní datové sady a datovou sadu výstupů.</span><span class="sxs-lookup"><span data-stu-id="a169e-529">There are two datasets: an input dataset and an output dataset.</span></span> <span data-ttu-id="a169e-530">Typ datové sady je nastaven na **AzureBlob** pro obojí.</span><span class="sxs-lookup"><span data-stu-id="a169e-530">The type of the dataset is set to **AzureBlob** for both.</span></span> 

<span data-ttu-id="a169e-531">Vstupní datová sada odkazuje na **vstupní** složky **adfblobconnector** kontejner objektů blob.</span><span class="sxs-lookup"><span data-stu-id="a169e-531">The input dataset points to the **input** folder of the **adfblobconnector** blob container.</span></span> <span data-ttu-id="a169e-532">**Externí** je nastavena na **true** pro tuto datovou sadu jako data není vytvořená v kanálu s aktivitou kopírování, která přebírá tuto datovou sadu jako vstup.</span><span class="sxs-lookup"><span data-stu-id="a169e-532">The **external** property is set to **true** for this dataset as the data is not produced by the pipeline with the copy activity that takes this dataset as an input.</span></span> 

<span data-ttu-id="a169e-533">Výstupní datovou sadu odkazuje na **výstup** složky stejné kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="a169e-533">The output dataset points to the **output** folder of the same blob container.</span></span> <span data-ttu-id="a169e-534">Výstupní datovou sadu také používá rok, měsíc a den **SliceStart** proměnné systému pro dynamicky vyhodnocení cesta pro výstupní soubor.</span><span class="sxs-lookup"><span data-stu-id="a169e-534">The output dataset also uses the year, month, and day of the **SliceStart** system variable to dynamically evaluate the path for the output file.</span></span> <span data-ttu-id="a169e-535">Seznam funkcí a systémové proměnné podporovaných službou Data Factory najdete v tématu [funkce pro vytváření dat a systémové proměnné](data-factory-functions-variables.md).</span><span class="sxs-lookup"><span data-stu-id="a169e-535">For a list of functions and system variables supported by Data Factory, see [Data Factory functions and system variables](data-factory-functions-variables.md).</span></span> <span data-ttu-id="a169e-536">**Externí** je nastavena na **false** (výchozí hodnota) protože tato datová sada je produkovaný kanálu.</span><span class="sxs-lookup"><span data-stu-id="a169e-536">The **external** property is set to **false** (default value) because this dataset is produced by the pipeline.</span></span> 

<span data-ttu-id="a169e-537">Další informace o vlastnostech podporovaných zprostředkovatelem datovou sadu objektu Blob Azure najdete v tématu [vlastnosti datové sady](#dataset-properties) části.</span><span class="sxs-lookup"><span data-stu-id="a169e-537">For more information about properties supported by Azure Blob dataset, see [Dataset properties](#dataset-properties) section.</span></span>

##### <a name="input-dataset"></a><span data-ttu-id="a169e-538">Vstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="a169e-538">Input dataset</span></span>

```json
{
    "name": "InputDataset-z4y",
    "properties": {
        "structure": [
            { "name": "Prop_0", "type": "String" },
            { "name": "Prop_1", "type": "String" }
        ],
        "type": "AzureBlob",
        "linkedServiceName": "Source-BlobStorage-z4y",
        "typeProperties": {
            "folderPath": "adfblobconnector/input/",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```

##### <a name="output-dataset"></a><span data-ttu-id="a169e-539">Výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="a169e-539">Output dataset</span></span>

```json
{
    "name": "OutputDataset-z4y",
    "properties": {
        "structure": [
            { "name": "Prop_0", "type": "String" },
            { "name": "Prop_1", "type": "String" }
        ],
        "type": "AzureBlob",
        "linkedServiceName": "Destination-BlobStorage-z4y",
        "typeProperties": {
            "folderPath": "adfblobconnector/output/{year}/{month}/{day}",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            },
            "partitionedBy": [
                { "name": "year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
                { "name": "month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
                { "name": "day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }
            ]
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        },
        "external": false,
        "policy": {}
    }
}
```

#### <a name="pipeline"></a><span data-ttu-id="a169e-540">Kanál</span><span class="sxs-lookup"><span data-stu-id="a169e-540">Pipeline</span></span>
<span data-ttu-id="a169e-541">Kanál obsahuje pouze jednu aktivitu.</span><span class="sxs-lookup"><span data-stu-id="a169e-541">The pipeline has just one activity.</span></span> <span data-ttu-id="a169e-542">**Typ** aktivity je nastaven na **kopie**.</span><span class="sxs-lookup"><span data-stu-id="a169e-542">The **type** of the activity is set to **Copy**.</span></span>  <span data-ttu-id="a169e-543">Ve vlastnostech typu aktivity jsou dvě části, jeden pro zdroj a jinou pro sink.</span><span class="sxs-lookup"><span data-stu-id="a169e-543">In the type properties for the activity, there are two sections, one for source and the other one for sink.</span></span> <span data-ttu-id="a169e-544">Typ zdroje je nastaven na **BlobSource** jako aktivity je kopírování dat z úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="a169e-544">The source type is set to **BlobSource** as the activity is copying data from a blob storage.</span></span> <span data-ttu-id="a169e-545">Typ jímky nastavena na **BlobSink** jako aktivita kopírování dat do úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="a169e-545">The sink type is set to **BlobSink** as the activity copying data to a blob storage.</span></span> <span data-ttu-id="a169e-546">Aktivita kopírování trvá InputDataset z4y jako vstup a OutputDataset z4y jako výstup.</span><span class="sxs-lookup"><span data-stu-id="a169e-546">The copy activity takes InputDataset-z4y as the input and OutputDataset-z4y as the output.</span></span> 

<span data-ttu-id="a169e-547">Další informace o vlastnostech podporovaných zprostředkovatelem BlobSource a BlobSink najdete v tématu [zkopírovat vlastnosti aktivity](#copy-activity-properties) části.</span><span class="sxs-lookup"><span data-stu-id="a169e-547">For more information about properties supported by BlobSource and BlobSink, see [Copy activity properties](#copy-activity-properties) section.</span></span> 

```json
{
    "name": "CopyPipeline",
    "properties": {
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource",
                        "recursive": false
                    },
                    "sink": {
                        "type": "BlobSink",
                        "copyBehavior": "MergeFiles",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataset-z4y"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataset-z4y"
                    }
                ],
                "policy": {
                    "timeout": "1.00:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "style": "StartOfInterval",
                    "retry": 3,
                    "longRetry": 0,
                    "longRetryInterval": "00:00:00"
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "Activity-0-Blob path_ adfblobconnector_input_->OutputDataset-z4y"
            }
        ],
        "start": "2017-04-21T22:34:00Z",
        "end": "2017-04-25T05:00:00Z",
        "isPaused": false,
        "pipelineMode": "Scheduled"
    }
}
```

## <a name="json-examples-for-copying-data-to-and-from-blob-storage"></a><span data-ttu-id="a169e-548">Příklady JSON pro kopírování dat do a z úložiště objektů Blob</span><span class="sxs-lookup"><span data-stu-id="a169e-548">JSON examples for copying data to and from Blob Storage</span></span>  
<span data-ttu-id="a169e-549">Následující příklady poskytují ukázka JSON definice, které můžete použít k vytvoření kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="a169e-549">The following examples provide sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="a169e-550">Se ukazují, jak ke zkopírování dat do a z Azure Blob Storage a Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="a169e-550">They show how to copy data to and from Azure Blob Storage and Azure SQL Database.</span></span> <span data-ttu-id="a169e-551">Nicméně je možné zkopírovat data **přímo** ze všech zdrojů do jakéhokoli z jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí aktivity kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="a169e-551">However, data can be copied **directly** from any of sources to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>

### <a name="json-example-copy-data-from-blob-storage-to-sql-database"></a><span data-ttu-id="a169e-552">Příklad JSON: Kopírování dat z úložiště objektů Blob do databáze SQL</span><span class="sxs-lookup"><span data-stu-id="a169e-552">JSON Example: Copy data from Blob Storage to SQL Database</span></span>
<span data-ttu-id="a169e-553">Následující příklad ukazuje:</span><span class="sxs-lookup"><span data-stu-id="a169e-553">The following sample shows:</span></span>

1. <span data-ttu-id="a169e-554">Propojené služby typu [azuresqldatabase](data-factory-azure-sql-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="a169e-554">A linked service of type [AzureSqlDatabase](data-factory-azure-sql-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="a169e-555">Propojené služby typu [azurestorage](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="a169e-555">A linked service of type [AzureStorage](#linked-service-properties).</span></span>
3. <span data-ttu-id="a169e-556">Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="a169e-556">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](#dataset-properties).</span></span>
4. <span data-ttu-id="a169e-557">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="a169e-557">An output [dataset](data-factory-create-datasets.md) of type [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="a169e-558">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [BlobSource](#copy-activity-properties) a [SqlSink](data-factory-azure-sql-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="a169e-558">A [pipeline](data-factory-create-pipelines.md) with a Copy activity that uses [BlobSource](#copy-activity-properties) and [SqlSink](data-factory-azure-sql-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="a169e-559">Kopie ukázka časové řady dat z Azure blob do Azure SQL tabulky každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="a169e-559">The sample copies time-series data from an Azure blob to an Azure SQL table hourly.</span></span> <span data-ttu-id="a169e-560">Vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="a169e-560">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="a169e-561">**Azure SQL propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="a169e-561">**Azure SQL linked service:**</span></span>

```json
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
<span data-ttu-id="a169e-562">**Propojená služba Azure Storage:**</span><span class="sxs-lookup"><span data-stu-id="a169e-562">**Azure Storage linked service:**</span></span>

```json
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
<span data-ttu-id="a169e-563">Podporuje dva typy Azure Storage, propojené služby Azure Data Factory: **azurestorage** a **AzureStorageSas**.</span><span class="sxs-lookup"><span data-stu-id="a169e-563">Azure Data Factory supports two types of Azure Storage linked services: **AzureStorage** and **AzureStorageSas**.</span></span> <span data-ttu-id="a169e-564">Pro první zadejte připojovací řetězec, který obsahuje klíč účtu a pro novější verzi, zadejte identifikátor Uri sdíleného přístupového podpisu (SAS).</span><span class="sxs-lookup"><span data-stu-id="a169e-564">For the first one, you specify the connection string that includes the account key and for the later one, you specify the Shared Access Signature (SAS) Uri.</span></span> <span data-ttu-id="a169e-565">V tématu [propojené služby](#linked-service-properties) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="a169e-565">See [Linked Services](#linked-service-properties) section for details.</span></span>  

<span data-ttu-id="a169e-566">**Azure vstupní datovou sadu objektu Blob:**</span><span class="sxs-lookup"><span data-stu-id="a169e-566">**Azure Blob input dataset:**</span></span>

<span data-ttu-id="a169e-567">Data je převzata z nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="a169e-567">Data is picked up from a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="a169e-568">Název složky a cesta k souboru pro tento objekt blob se vyhodnocují dynamicky podle času zahájení řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="a169e-568">The folder path and file name for the blob are dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="a169e-569">Cesta ke složce používá rok, měsíc a den součástí čas spuštění a název souboru používá hodinu součástí čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="a169e-569">The folder path uses year, month, and day part of the start time and file name uses the hour part of the start time.</span></span> <span data-ttu-id="a169e-570">"externí": "PRAVDA" nastavení informuje objekt pro vytváření dat, v tabulce je externí k objektu pro vytváření dat a není vyprodukované aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="a169e-570">“external”: “true” setting informs Data Factory that the table is external to the data factory and is not produced by an activity in the data factory.</span></span>

```json
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/",
      "fileName": "{Hour}.csv",
      "partitionedBy": [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "HH" } }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": "\n"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```
<span data-ttu-id="a169e-571">**Azure SQL výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="a169e-571">**Azure SQL output dataset:**</span></span>

<span data-ttu-id="a169e-572">Ukázková data kopie tabulku s názvem "MyTable" v Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="a169e-572">The sample copies data to a table named “MyTable” in an Azure SQL database.</span></span> <span data-ttu-id="a169e-573">Vytvoření tabulky ve vaší databázi Azure SQL s stejný počet sloupců, podle očekávání souboru CSV objektů Blob tak, aby obsahovala.</span><span class="sxs-lookup"><span data-stu-id="a169e-573">Create the table in your Azure SQL database with the same number of columns as you expect the Blob CSV file to contain.</span></span> <span data-ttu-id="a169e-574">Nové záznamy se přidají do tabulky každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="a169e-574">New rows are added to the table every hour.</span></span>

```json
{
  "name": "AzureSqlOutput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
    "typeProperties": {
      "tableName": "MyOutputTable"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
<span data-ttu-id="a169e-575">**Aktivita kopírování v kanálu s Blob zdroj a jímka SQL:**</span><span class="sxs-lookup"><span data-stu-id="a169e-575">**A copy activity in a pipeline with Blob source and SQL sink:**</span></span>

<span data-ttu-id="a169e-576">Kanál obsahuje aktivitu kopírování, který je nakonfigurovaný na použití vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="a169e-576">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="a169e-577">V definici JSON kanálu **zdroj** je typ nastaven na **BlobSource** a **podřízený** je typ nastaven na **SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="a169e-577">In the pipeline JSON definition, the **source** type is set to **BlobSource** and **sink** type is set to **SqlSink**.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoSQL",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "SqlSink"
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
### <a name="json-example-copy-data-from-azure-sql-to-azure-blob"></a><span data-ttu-id="a169e-578">Příklad JSON: Kopírování dat z Azure SQL do Azure Blob</span><span class="sxs-lookup"><span data-stu-id="a169e-578">JSON Example: Copy data from Azure SQL to Azure Blob</span></span>
<span data-ttu-id="a169e-579">Následující příklad ukazuje:</span><span class="sxs-lookup"><span data-stu-id="a169e-579">The following sample shows:</span></span>

1. <span data-ttu-id="a169e-580">Propojené služby typu [azuresqldatabase](data-factory-azure-sql-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="a169e-580">A linked service of type [AzureSqlDatabase](data-factory-azure-sql-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="a169e-581">Propojené služby typu [azurestorage](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="a169e-581">A linked service of type [AzureStorage](#linked-service-properties).</span></span>
3. <span data-ttu-id="a169e-582">Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="a169e-582">An input [dataset](data-factory-create-datasets.md) of type [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="a169e-583">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="a169e-583">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](#dataset-properties).</span></span>
5. <span data-ttu-id="a169e-584">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [SqlSource](data-factory-azure-sql-connector.md#copy-activity-properties) a [BlobSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="a169e-584">A [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [SqlSource](data-factory-azure-sql-connector.md#copy-activity-properties) and [BlobSink](#copy-activity-properties).</span></span>

<span data-ttu-id="a169e-585">Ukázka kopíruje data časové řady z tabulky Azure SQL do objektu blob Azure každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="a169e-585">The sample copies time-series data from an Azure SQL table to an Azure blob hourly.</span></span> <span data-ttu-id="a169e-586">Vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="a169e-586">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="a169e-587">**Azure SQL propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="a169e-587">**Azure SQL linked service:**</span></span>

```json
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
<span data-ttu-id="a169e-588">**Propojená služba Azure Storage:**</span><span class="sxs-lookup"><span data-stu-id="a169e-588">**Azure Storage linked service:**</span></span>

```json
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
<span data-ttu-id="a169e-589">Podporuje dva typy Azure Storage, propojené služby Azure Data Factory: **azurestorage** a **AzureStorageSas**.</span><span class="sxs-lookup"><span data-stu-id="a169e-589">Azure Data Factory supports two types of Azure Storage linked services: **AzureStorage** and **AzureStorageSas**.</span></span> <span data-ttu-id="a169e-590">Pro první zadejte připojovací řetězec, který obsahuje klíč účtu a pro novější verzi, zadejte identifikátor Uri sdíleného přístupového podpisu (SAS).</span><span class="sxs-lookup"><span data-stu-id="a169e-590">For the first one, you specify the connection string that includes the account key and for the later one, you specify the Shared Access Signature (SAS) Uri.</span></span> <span data-ttu-id="a169e-591">V tématu [propojené služby](#linked-service-properties) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="a169e-591">See [Linked Services](#linked-service-properties) section for details.</span></span>  

<span data-ttu-id="a169e-592">**Azure SQL vstupní datové sady:**</span><span class="sxs-lookup"><span data-stu-id="a169e-592">**Azure SQL input dataset:**</span></span>

<span data-ttu-id="a169e-593">Příkladu se předpokládá, jste vytvořili tabulku "MyTable" v Azure SQL a obsahuje sloupec s názvem "timestampcolumn" pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="a169e-593">The sample assumes you have created a table “MyTable” in Azure SQL and it contains a column called “timestampcolumn” for time series data.</span></span>

<span data-ttu-id="a169e-594">Nastavení "externí": "PRAVDA" informuje služba Data Factory, v tabulce je externí k objektu pro vytváření dat a není vyprodukované aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="a169e-594">Setting “external”: ”true” informs Data Factory service that the table is external to the data factory and is not produced by an activity in the data factory.</span></span>

```json
{
  "name": "AzureSqlInput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
    "typeProperties": {
      "tableName": "MyTable"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```

<span data-ttu-id="a169e-595">**Azure Blob výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="a169e-595">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="a169e-596">Data se zapisují do nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="a169e-596">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="a169e-597">Cesta ke složce pro tento objekt blob je vyhodnocován dynamicky podle času zahájení řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="a169e-597">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="a169e-598">Cesta ke složce používá rok, měsíc, den a čas částí čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="a169e-598">The folder path uses year, month, day, and hours parts of the start time.</span></span>

```json
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}/",
      "partitionedBy": [
        {
          "name": "Year",
          "value": { "type": "DateTime",  "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "HH" } }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": "\t",
        "rowDelimiter": "\n"
      }
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

<span data-ttu-id="a169e-599">**Aktivita kopírování v kanálu s SQL zdrojový a podřízený objekt Blob:**</span><span class="sxs-lookup"><span data-stu-id="a169e-599">**A copy activity in a pipeline with SQL source and Blob sink:**</span></span>

<span data-ttu-id="a169e-600">Kanál obsahuje aktivitu kopírování, který je nakonfigurovaný na použití vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="a169e-600">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="a169e-601">V definici JSON kanálu **zdroj** je typ nastaven na **SqlSource** a **podřízený** je typ nastaven na **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="a169e-601">In the pipeline JSON definition, the **source** type is set to **SqlSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="a169e-602">Zadané pro dotaz SQL **SqlReaderQuery** vlastnost vybere data za poslední hodinu pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="a169e-602">The SQL query specified for the **SqlReaderQuery** property selects the data in the past hour to copy.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
              {
                "name": "AzureSQLtoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "AzureSQLInput"
                  }
                ],
                "outputs": [
                  {
                    "name": "AzureBlobOutput"
                  }
                ],
                "typeProperties": {
                    "source": {
                        "type": "SqlSource",
                        "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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

> [!NOTE]
> <span data-ttu-id="a169e-603">Mapování sloupců z datové sady zdroje na sloupce ze sady jímku dat naleznete v tématu [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="a169e-603">To map columns from source dataset to columns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="a169e-604">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="a169e-604">Performance and Tuning</span></span>
<span data-ttu-id="a169e-605">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) Další informace o klíčových faktorů, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby, jak optimalizovat ho.</span><span class="sxs-lookup"><span data-stu-id="a169e-605">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
