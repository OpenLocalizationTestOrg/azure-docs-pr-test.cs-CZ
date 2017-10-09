---
title: aaaCopy data do/z Azure Blob Storage | Microsoft Docs
description: "Zjistěte, jak toocopy blob dat v Azure Data Factory. Použijte naše ukázka: jak toocopy tooand dat z Azure Blob Storage a Azure SQL Database."
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
ms.openlocfilehash: 8428c64e8e8b1084b3f2f680c4e1819559e4ffa3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tooor-from-azure-blob-storage-using-azure-data-factory"></a><span data-ttu-id="61244-105">Tooor kopírování dat z Azure Blob Storage pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="61244-105">Copy data tooor from Azure Blob Storage using Azure Data Factory</span></span>
<span data-ttu-id="61244-106">Tento článek vysvětluje, jak toouse hello aktivitu kopírování v Azure Data Factory toocopy data tooand z Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="61244-106">This article explains how toouse hello Copy Activity in Azure Data Factory toocopy data tooand from Azure Blob Storage.</span></span> <span data-ttu-id="61244-107">Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="61244-107">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

## <a name="overview"></a><span data-ttu-id="61244-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="61244-108">Overview</span></span>
<span data-ttu-id="61244-109">Data můžete zkopírovat z jakéhokoli podporované zdroje dat ukládání tooAzure úložiště objektů Blob nebo z Azure Blob Storage tooany podporované jímku dat úložiště.</span><span class="sxs-lookup"><span data-stu-id="61244-109">You can copy data from any supported source data store tooAzure Blob Storage or from Azure Blob Storage tooany supported sink data store.</span></span> <span data-ttu-id="61244-110">Hello následující tabulka obsahuje seznam úložiště dat, které jsou podporované jako zdroje nebo jímky aktivitou kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="61244-110">hello following table provides a list of data stores supported as sources or sinks by hello copy activity.</span></span> <span data-ttu-id="61244-111">Například můžete přesunout data **z** databázi systému SQL Server nebo Azure SQL database **k** Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="61244-111">For example, you can move data **from** a SQL Server database or an Azure SQL database **to** an Azure blob storage.</span></span> <span data-ttu-id="61244-112">A může kopírovat data **z** úložiště objektů blob Azure **k** Azure SQL Data Warehouse nebo kolekci Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="61244-112">And, you can copy data **from** Azure blob storage **to** an Azure SQL Data Warehouse or an Azure Cosmos DB collection.</span></span> 

## <a name="supported-scenarios"></a><span data-ttu-id="61244-113">Podporované scénáře</span><span class="sxs-lookup"><span data-stu-id="61244-113">Supported scenarios</span></span>
<span data-ttu-id="61244-114">Může kopírovat data **z Azure Blob Storage** toohello následující úložišť dat:</span><span class="sxs-lookup"><span data-stu-id="61244-114">You can copy data **from Azure Blob Storage** toohello following data stores:</span></span>

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="61244-115">Data můžete zkopírovat z hello následující úložišť dat **tooAzure úložiště objektů Blob**:</span><span class="sxs-lookup"><span data-stu-id="61244-115">You can copy data from hello following data stores **tooAzure Blob Storage**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]
 
> [!IMPORTANT]
> <span data-ttu-id="61244-116">Aktivita kopírování podporuje kopírování dat z / tooboth účtů úložiště pro obecné účely Azure a Hot/Cool úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="61244-116">Copy Activity supports copying data from/tooboth general-purpose Azure Storage accounts and Hot/Cool Blob storage.</span></span> <span data-ttu-id="61244-117">Aktivita Hello podporuje **čtení z bloku, připojte, nebo objekty BLOB stránky**, ale podporuje **zápis objekty BLOB bloku tooonly**.</span><span class="sxs-lookup"><span data-stu-id="61244-117">hello activity supports **reading from block, append, or page blobs**, but supports **writing tooonly block blobs**.</span></span> <span data-ttu-id="61244-118">Azure Storage úrovně Premium není podporován jako jímka, protože je zálohovaný díky objekty BLOB stránky.</span><span class="sxs-lookup"><span data-stu-id="61244-118">Azure Premium Storage is not supported as a sink because it is backed by page blobs.</span></span>
> 
> <span data-ttu-id="61244-119">Aktivita kopírování nedojde k odstranění dat ze zdroje hello po hello, data se úspěšně zkopíroval toohello cílový.</span><span class="sxs-lookup"><span data-stu-id="61244-119">Copy Activity does not delete data from hello source after hello data is successfully copied toohello destination.</span></span> <span data-ttu-id="61244-120">Pokud potřebujete po úspěšné kopie toodelete zdrojová data, vytvořte [vlastní aktivity](data-factory-use-custom-activities.md) toodelete hello dat a používání hello aktivitu v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="61244-120">If you need toodelete source data after a successful copy, create a [custom activity](data-factory-use-custom-activities.md) toodelete hello data and use hello activity in hello pipeline.</span></span> <span data-ttu-id="61244-121">Příklad najdete v tématu hello [odstranění objektů blob nebo složky ukázce na Githubu](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/DeleteBlobFileFolderCustomActivity).</span><span class="sxs-lookup"><span data-stu-id="61244-121">For an example, see hello [Delete blob or folder sample on GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/DeleteBlobFileFolderCustomActivity).</span></span> 

## <a name="get-started"></a><span data-ttu-id="61244-122">Začínáme</span><span class="sxs-lookup"><span data-stu-id="61244-122">Get started</span></span>
<span data-ttu-id="61244-123">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z Azure Blob Storage pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="61244-123">You can create a pipeline with a copy activity that moves data to/from an Azure Blob Storage by using different tools/APIs.</span></span>

<span data-ttu-id="61244-124">Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="61244-124">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="61244-125">Tento článek má [návod](#walkthrough-use-copy-wizard-to-copy-data-tofrom-blob-storage) pro vytváření dat kanál toocopy ze Azure Blob Storage umístění tooanother umístění úložiště objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="61244-125">This article has a [walkthrough](#walkthrough-use-copy-wizard-to-copy-data-tofrom-blob-storage) for creating a pipeline toocopy data from an Azure Blob Storage location  tooanother Azure Blob Storage location.</span></span> <span data-ttu-id="61244-126">Kurz týkající se vytváření kanálu toocopy data ze Azure Blob Storage tooAzure SQL Database, najdete v části [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="61244-126">For a tutorial on creating a pipeline toocopy data from an Azure Blob Storage tooAzure SQL Database, see [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md).</span></span>

<span data-ttu-id="61244-127">Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**.</span><span class="sxs-lookup"><span data-stu-id="61244-127">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="61244-128">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="61244-128">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span>

<span data-ttu-id="61244-129">Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:</span><span class="sxs-lookup"><span data-stu-id="61244-129">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="61244-130">Vytvoření **objekt pro vytváření dat**.</span><span class="sxs-lookup"><span data-stu-id="61244-130">Create a **data factory**.</span></span> <span data-ttu-id="61244-131">Objekt pro vytváření dat může obsahovat jeden nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="61244-131">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="61244-132">Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="61244-132">Create **linked services** toolink input and output data stores tooyour data factory.</span></span> <span data-ttu-id="61244-133">Například pokud kopírování dat z Azure SQL database tooan úložiště objektů blob v Azure, vytvoříte dvě propojené služby toolink vašeho účtu úložiště Azure a Azure SQL databáze tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="61244-133">For example, if you are copying data from an Azure blob storage tooan Azure SQL database, you create two linked services toolink your Azure storage account and Azure SQL database tooyour data factory.</span></span> <span data-ttu-id="61244-134">Vlastnosti propojené služby, které jsou specifické tooAzure úložiště objektů Blob, najdete v části [propojené vlastnosti služby](#linked-service-properties) části.</span><span class="sxs-lookup"><span data-stu-id="61244-134">For linked service properties that are specific tooAzure Blob Storage, see [linked service properties](#linked-service-properties) section.</span></span> 
2. <span data-ttu-id="61244-135">Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="61244-135">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> <span data-ttu-id="61244-136">V příkladu hello uvedených v posledním kroku hello vytvořte kontejner objektů blob hello toospecify datovou sadu a složky, která obsahuje vstupní data hello.</span><span class="sxs-lookup"><span data-stu-id="61244-136">In hello example mentioned in hello last step, you create a dataset toospecify hello blob container and folder that contains hello input data.</span></span> <span data-ttu-id="61244-137">A vytvořte jinou datovou sadu toospecify hello SQL tabulkou v hello Azure SQL database, která obsahuje hello daty zkopírovanými z úložiště objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="61244-137">And, you create another dataset toospecify hello SQL table in hello Azure SQL database that holds hello data copied from hello blob storage.</span></span> <span data-ttu-id="61244-138">Vlastnosti datové sady, které jsou specifické tooAzure úložiště objektů Blob, najdete v části [vlastnosti datové sady](#dataset-properties) části.</span><span class="sxs-lookup"><span data-stu-id="61244-138">For dataset properties that are specific tooAzure Blob Storage, see [dataset properties](#dataset-properties) section.</span></span>
3. <span data-ttu-id="61244-139">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="61244-139">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="61244-140">V příkladu hello již bylo zmíněno dříve použijete BlobSource jako zdroj a SqlSink jako jímku pro aktivitu kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="61244-140">In hello example mentioned earlier, you use BlobSource as a source and SqlSink as a sink for hello copy activity.</span></span> <span data-ttu-id="61244-141">Podobně pokud zkopírujete z Azure SQL Database tooAzure úložiště objektů Blob, použijte SqlSource a BlobSink v aktivitě kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="61244-141">Similarly, if you are copying from Azure SQL Database tooAzure Blob Storage, you use SqlSource and BlobSink in hello copy activity.</span></span> <span data-ttu-id="61244-142">Vlastnosti aktivity kopírování, které jsou specifické tooAzure úložiště objektů Blob, najdete v části [zkopírovat vlastnosti aktivity](#copy-activity-properties) části.</span><span class="sxs-lookup"><span data-stu-id="61244-142">For copy activity properties that are specific tooAzure Blob Storage, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="61244-143">Podrobnosti o způsobu toouse úložiště dat jako zdroj nebo jímka klikněte na tlačítko hello odkaz v předchozí části hello pro data store.</span><span class="sxs-lookup"><span data-stu-id="61244-143">For details on how toouse a data store as a source or a sink, click hello link in hello previous section for your data store.</span></span>  

<span data-ttu-id="61244-144">Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello).</span><span class="sxs-lookup"><span data-stu-id="61244-144">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="61244-145">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.</span><span class="sxs-lookup"><span data-stu-id="61244-145">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="61244-146">Ukázky s definicemi JSON entit služby Data Factory, které jsou používané toocopy data do/z Azure Blob Storage, najdete v části [JSON příklady](#json-examples-for-copying-data-to-and-from-blob-storage  ) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="61244-146">For samples with JSON definitions for Data Factory entities that are used toocopy data to/from an Azure Blob Storage, see [JSON examples](#json-examples-for-copying-data-to-and-from-blob-storage  ) section of this article.</span></span>

<span data-ttu-id="61244-147">Hello následující části obsahují podrobné informace o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooAzure úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="61244-147">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooAzure Blob Storage.</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="61244-148">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="61244-148">Linked service properties</span></span>
<span data-ttu-id="61244-149">Existují dva typy propojené služby můžete použít toolink služby Azure Storage tooan Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="61244-149">There are two types of linked services you can use toolink an Azure Storage tooan Azure data factory.</span></span> <span data-ttu-id="61244-150">Jsou: **azurestorage** propojená služba a **AzureStorageSas** propojené služby.</span><span class="sxs-lookup"><span data-stu-id="61244-150">They are: **AzureStorage** linked service and **AzureStorageSas** linked service.</span></span> <span data-ttu-id="61244-151">Hello propojené služby Azure Storage poskytuje objekt pro vytváření dat hello s globálním přístupem toohello Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="61244-151">hello Azure Storage linked service provides hello data factory with global access toohello Azure Storage.</span></span> <span data-ttu-id="61244-152">Zatímco hello Azure úložiště SAS (sdíleného přístupového podpisu) propojená služba poskytuje objekt pro vytváření dat hello s přístup omezený nebo časově vázaných toohello Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="61244-152">Whereas, hello Azure Storage SAS (Shared Access Signature) linked service provides hello data factory with restricted/time-bound access toohello Azure Storage.</span></span> <span data-ttu-id="61244-153">Nejsou žádné další rozdíly mezi tyto dvě propojené služby.</span><span class="sxs-lookup"><span data-stu-id="61244-153">There are no other differences between these two linked services.</span></span> <span data-ttu-id="61244-154">Zvolte hello propojené služby, který vyhovuje vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="61244-154">Choose hello linked service that suits your needs.</span></span> <span data-ttu-id="61244-155">Hello následující oddíly poskytují další podrobnosti o tyto dvě propojené služby.</span><span class="sxs-lookup"><span data-stu-id="61244-155">hello following sections provide more details on these two linked services.</span></span>

[!INCLUDE [data-factory-azure-storage-linked-services](../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="dataset-properties"></a><span data-ttu-id="61244-156">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="61244-156">Dataset properties</span></span>
<span data-ttu-id="61244-157">toospecify toorepresent datové sady vstupních nebo výstupních dat v Azure Blob Storage, nastavte vlastnost typu hello hello datovou sadu, která: **AzureBlob**.</span><span class="sxs-lookup"><span data-stu-id="61244-157">toospecify a dataset toorepresent input or output data in an Azure Blob Storage, you set hello type property of hello dataset to: **AzureBlob**.</span></span> <span data-ttu-id="61244-158">Sada hello **linkedServiceName** vlastnost hello datovou sadu toohello název hello Azure Storage nebo Azure úložiště SAS propojené služby.</span><span class="sxs-lookup"><span data-stu-id="61244-158">Set hello **linkedServiceName** property of hello dataset toohello name of hello Azure Storage or Azure Storage SAS linked service.</span></span>  <span data-ttu-id="61244-159">vlastnosti typu Hello sady dat hello zadejte hello **kontejner objektů blob** a hello **složky** v úložišti objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="61244-159">hello type properties of hello dataset specify hello **blob container** and hello **folder** in hello blob storage.</span></span>

<span data-ttu-id="61244-160">Úplný seznam části JSON & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="61244-160">For a full list of JSON sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="61244-161">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="61244-161">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="61244-162">Objekt pro vytváření dat podporuje následující hodnoty kompatibilní se specifikací CLS .NET na základě typu pro poskytnutí informací o typu "struktury" zdroje dat schématu na čtení jako Azure blob hello: Int16, Int32, Int64, jeden, Double, Decimal, Byte [], Bool, String, Guid, Datetime, DateTimeOffset, Timespan.</span><span class="sxs-lookup"><span data-stu-id="61244-162">Data factory supports hello following CLS-compliant .NET based type values for providing type information in “structure” for schema-on-read data sources like Azure blob: Int16, Int32, Int64, Single, Double, Decimal, Byte[], Bool, String, Guid, Datetime, Datetimeoffset, Timespan.</span></span> <span data-ttu-id="61244-163">Objekt pro vytváření dat převody typů automaticky provede při přesunu, že data ze zdrojových dat úložiště tooa podřízený data.</span><span class="sxs-lookup"><span data-stu-id="61244-163">Data Factory automatically performs type conversions when moving data from a source data store tooa sink data store.</span></span>

<span data-ttu-id="61244-164">Hello **rámci typeProperties** oddílu se liší pro jednotlivé typy datovou sadu a informace o umístění hello formátu atd, hello dat v úložišti dat hello.</span><span class="sxs-lookup"><span data-stu-id="61244-164">hello **typeProperties** section is different for each type of dataset and provides information about hello location, format etc., of hello data in hello data store.</span></span> <span data-ttu-id="61244-165">rámci typeProperties Hello část datové sady typ **AzureBlob** datová sada má hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="61244-165">hello typeProperties section for dataset of type **AzureBlob** dataset has hello following properties:</span></span>

| <span data-ttu-id="61244-166">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="61244-166">Property</span></span> | <span data-ttu-id="61244-167">Popis</span><span class="sxs-lookup"><span data-stu-id="61244-167">Description</span></span> | <span data-ttu-id="61244-168">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="61244-168">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="61244-169">folderPath</span><span class="sxs-lookup"><span data-stu-id="61244-169">folderPath</span></span> |<span data-ttu-id="61244-170">Cesta toohello kontejneru a složce v úložišti objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="61244-170">Path toohello container and folder in hello blob storage.</span></span> <span data-ttu-id="61244-171">Příklad: myblobcontainer\myblobfolder\\</span><span class="sxs-lookup"><span data-stu-id="61244-171">Example: myblobcontainer\myblobfolder\\</span></span> |<span data-ttu-id="61244-172">Ano</span><span class="sxs-lookup"><span data-stu-id="61244-172">Yes</span></span> |
| <span data-ttu-id="61244-173">fileName</span><span class="sxs-lookup"><span data-stu-id="61244-173">fileName</span></span> |<span data-ttu-id="61244-174">Název objektu hello blob.</span><span class="sxs-lookup"><span data-stu-id="61244-174">Name of hello blob.</span></span> <span data-ttu-id="61244-175">Název souboru je volitelné a velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="61244-175">fileName is optional and case-sensitive.</span></span><br/><br/><span data-ttu-id="61244-176">Pokud určíte název souboru, hello aktivitu (včetně kopie) funguje na hello konkrétní objekt Blob.</span><span class="sxs-lookup"><span data-stu-id="61244-176">If you specify a filename, hello activity (including Copy) works on hello specific Blob.</span></span><br/><br/><span data-ttu-id="61244-177">Pokud není zadán název souboru, zahrnuje kopírování všech objektů BLOB v hello folderPath pro vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="61244-177">When fileName is not specified, Copy includes all Blobs in hello folderPath for input dataset.</span></span><br/><br/><span data-ttu-id="61244-178">Když **fileName** pro datovou sadu výstupů není zadána a **preserveHierarchy** není zadané v aktivity podřízený hello název souboru hello generované by v hello následující tento formát: Data.<Guid>. TXT (například:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="61244-178">When **fileName** is not specified for an output dataset and **preserveHierarchy** is not specified in activity sink, hello name of hello generated file would be in hello following this format: Data.<Guid>.txt (for example: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="61244-179">Ne</span><span class="sxs-lookup"><span data-stu-id="61244-179">No</span></span> |
| <span data-ttu-id="61244-180">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="61244-180">partitionedBy</span></span> |<span data-ttu-id="61244-181">partitionedBy vlastnost je volitelná.</span><span class="sxs-lookup"><span data-stu-id="61244-181">partitionedBy is an optional property.</span></span> <span data-ttu-id="61244-182">Můžete ho toospecify dynamické folderPath a název souboru pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="61244-182">You can use it toospecify a dynamic folderPath and filename for time series data.</span></span> <span data-ttu-id="61244-183">Například folderPath lze nastavit parametry pro každou hodinu data.</span><span class="sxs-lookup"><span data-stu-id="61244-183">For example, folderPath can be parameterized for every hour of data.</span></span> <span data-ttu-id="61244-184">V tématu hello [pomocí části vlastnost partitionedBy](#using-partitionedBy-property) podrobnosti a příklady.</span><span class="sxs-lookup"><span data-stu-id="61244-184">See hello [Using partitionedBy property section](#using-partitionedBy-property) for details and examples.</span></span> |<span data-ttu-id="61244-185">Ne</span><span class="sxs-lookup"><span data-stu-id="61244-185">No</span></span> |
| <span data-ttu-id="61244-186">Formát</span><span class="sxs-lookup"><span data-stu-id="61244-186">format</span></span> | <span data-ttu-id="61244-187">jsou podporovány následující typy formátu Hello: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="61244-187">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="61244-188">Sada hello **typ** vlastnost pod formátu tooone z těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="61244-188">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="61244-189">Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly.</span><span class="sxs-lookup"><span data-stu-id="61244-189">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="61244-190">Pokud chcete příliš**zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu hello v obou definice vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="61244-190">If you want too**copy files as-is** between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="61244-191">Ne</span><span class="sxs-lookup"><span data-stu-id="61244-191">No</span></span> |
| <span data-ttu-id="61244-192">Komprese</span><span class="sxs-lookup"><span data-stu-id="61244-192">compression</span></span> | <span data-ttu-id="61244-193">Zadejte typ hello a úroveň komprese dat hello.</span><span class="sxs-lookup"><span data-stu-id="61244-193">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="61244-194">Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="61244-194">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="61244-195">Jsou podporované úrovně: **Optimal** a **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="61244-195">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="61244-196">Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="61244-196">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="61244-197">Ne</span><span class="sxs-lookup"><span data-stu-id="61244-197">No</span></span> |

### <a name="using-partitionedby-property"></a><span data-ttu-id="61244-198">Pomocí vlastnost partitionedBy</span><span class="sxs-lookup"><span data-stu-id="61244-198">Using partitionedBy property</span></span>
<span data-ttu-id="61244-199">Jak je uvedeno v předchozí části hello, můžete zadat dynamické folderPath a název souboru pro data časové řady s hello **partitionedBy** vlastnost [funkce pro vytváření dat a systémové proměnné hello](data-factory-functions-variables.md).</span><span class="sxs-lookup"><span data-stu-id="61244-199">As mentioned in hello previous section, you can specify a dynamic folderPath and filename for time series data with hello **partitionedBy** property, [Data Factory functions, and hello system variables](data-factory-functions-variables.md).</span></span>

<span data-ttu-id="61244-200">Další informace o datové sady času řady, plánování a řezy najdete v tématu [vytváření datových sad](data-factory-create-datasets.md) a [plánování a provádění](data-factory-scheduling-and-execution.md) články.</span><span class="sxs-lookup"><span data-stu-id="61244-200">For more information on time series datasets, scheduling, and slices, see [Creating Datasets](data-factory-create-datasets.md) and [Scheduling & Execution](data-factory-scheduling-and-execution.md) articles.</span></span>

#### <a name="sample-1"></a><span data-ttu-id="61244-201">Ukázka 1</span><span class="sxs-lookup"><span data-stu-id="61244-201">Sample 1</span></span>

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

<span data-ttu-id="61244-202">V tomto příkladu {řez} se nahradí hello hodnotu objektu pro vytváření dat systému proměnné SliceStart ve formátu hello (YYYYMMDDHH) zadán.</span><span class="sxs-lookup"><span data-stu-id="61244-202">In this example, {Slice} is replaced with hello value of Data Factory system variable SliceStart in hello format (YYYYMMDDHH) specified.</span></span> <span data-ttu-id="61244-203">Hello SliceStart odkazuje toostart času řezu hello.</span><span class="sxs-lookup"><span data-stu-id="61244-203">hello SliceStart refers toostart time of hello slice.</span></span> <span data-ttu-id="61244-204">Hello folderPath se liší pro každý řez.</span><span class="sxs-lookup"><span data-stu-id="61244-204">hello folderPath is different for each slice.</span></span> <span data-ttu-id="61244-205">Příklad: wikidatagateway/wikisampledataout/2014100103 nebo wikidatagateway/wikisampledataout/2014100104</span><span class="sxs-lookup"><span data-stu-id="61244-205">For example: wikidatagateway/wikisampledataout/2014100103 or wikidatagateway/wikisampledataout/2014100104</span></span>

#### <a name="sample-2"></a><span data-ttu-id="61244-206">Ukázka 2</span><span class="sxs-lookup"><span data-stu-id="61244-206">Sample 2</span></span>

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

<span data-ttu-id="61244-207">V tomto příkladu jsou extrahován rok, měsíc, den a čas SliceStart do samostatné proměnné, které jsou používány folderPath a název vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="61244-207">In this example, year, month, day, and time of SliceStart are extracted into separate variables that are used by folderPath and fileName properties.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="61244-208">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="61244-208">Copy activity properties</span></span>
<span data-ttu-id="61244-209">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="61244-209">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="61244-210">Vlastnosti, například název, popis, vstupní a výstupní datové sady a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="61244-210">Properties such as name, description, input and output datasets, and policies are available for all types of activities.</span></span> <span data-ttu-id="61244-211">Vzhledem k tomu, vlastnosti dostupné ve hello **rámci typeProperties** části hello aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="61244-211">Whereas, properties available in hello **typeProperties** section of hello activity vary with each activity type.</span></span> <span data-ttu-id="61244-212">Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="61244-212">For Copy activity, they vary depending on hello types of sources and sinks.</span></span> <span data-ttu-id="61244-213">Pokud přesouváte data z Azure Blob Storage, nastavíte typ zdroje hello v aktivitě kopírování hello příliš**BlobSource**.</span><span class="sxs-lookup"><span data-stu-id="61244-213">If you are moving data from an Azure Blob Storage, you set hello source type in hello copy activity too**BlobSource**.</span></span> <span data-ttu-id="61244-214">Podobně pokud přesouváte data tooan Azure Blob Storage, nastavíte typ jímky hello v aktivitě kopírování hello příliš**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="61244-214">Similarly, if you are moving data tooan Azure Blob Storage, you set hello sink type in hello copy activity too**BlobSink**.</span></span> <span data-ttu-id="61244-215">Tato část obsahuje seznam vlastností, které jsou podporované BlobSource a BlobSink.</span><span class="sxs-lookup"><span data-stu-id="61244-215">This section provides a list of properties supported by BlobSource and BlobSink.</span></span>

<span data-ttu-id="61244-216">**BlobSource** podporuje následující vlastnosti v hello hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="61244-216">**BlobSource** supports hello following properties in hello **typeProperties** section:</span></span>

| <span data-ttu-id="61244-217">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="61244-217">Property</span></span> | <span data-ttu-id="61244-218">Popis</span><span class="sxs-lookup"><span data-stu-id="61244-218">Description</span></span> | <span data-ttu-id="61244-219">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="61244-219">Allowed values</span></span> | <span data-ttu-id="61244-220">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="61244-220">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="61244-221">Rekurzivní</span><span class="sxs-lookup"><span data-stu-id="61244-221">recursive</span></span> |<span data-ttu-id="61244-222">Určuje, zda text hello je číst data rekurzivně z hello podsložek nebo pouze z hello zadané složky.</span><span class="sxs-lookup"><span data-stu-id="61244-222">Indicates whether hello data is read recursively from hello sub folders or only from hello specified folder.</span></span> |<span data-ttu-id="61244-223">True (výchozí hodnota), False.</span><span class="sxs-lookup"><span data-stu-id="61244-223">True (default value), False</span></span> |<span data-ttu-id="61244-224">Ne</span><span class="sxs-lookup"><span data-stu-id="61244-224">No</span></span> |

<span data-ttu-id="61244-225">**BlobSink** podporuje následující vlastnosti hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="61244-225">**BlobSink** supports hello following properties **typeProperties** section:</span></span>

| <span data-ttu-id="61244-226">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="61244-226">Property</span></span> | <span data-ttu-id="61244-227">Popis</span><span class="sxs-lookup"><span data-stu-id="61244-227">Description</span></span> | <span data-ttu-id="61244-228">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="61244-228">Allowed values</span></span> | <span data-ttu-id="61244-229">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="61244-229">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="61244-230">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="61244-230">copyBehavior</span></span> |<span data-ttu-id="61244-231">Definuje chování kopie hello, pokud je zdroj hello BlobSource nebo systému souborů.</span><span class="sxs-lookup"><span data-stu-id="61244-231">Defines hello copy behavior when hello source is BlobSource or FileSystem.</span></span> |<span data-ttu-id="61244-232"><b>PreserveHierarchy</b>: uchovává hello hierarchií souborů v cílové složce hello.</span><span class="sxs-lookup"><span data-stu-id="61244-232"><b>PreserveHierarchy</b>: preserves hello file hierarchy in hello target folder.</span></span> <span data-ttu-id="61244-233">relativní cesta Hello zdrojové složky toosource souboru je identické toohello relativní cestu složky tootarget cílového souboru.</span><span class="sxs-lookup"><span data-stu-id="61244-233">hello relative path of source file toosource folder is identical toohello relative path of target file tootarget folder.</span></span><br/><br/><span data-ttu-id="61244-234"><b>FlattenHierarchy</b>: všechny soubory ze zdrojové složky hello jsou v hello první úroveň cílové složce.</span><span class="sxs-lookup"><span data-stu-id="61244-234"><b>FlattenHierarchy</b>: all files from hello source folder are in hello first level of target folder.</span></span> <span data-ttu-id="61244-235">Hello zaměřením mít název automaticky generovány.</span><span class="sxs-lookup"><span data-stu-id="61244-235">hello target files have auto generated name.</span></span> <br/><br/><span data-ttu-id="61244-236"><b>MergeFiles</b>: sloučí všechny soubory ze hello zdrojové složky tooone souboru.</span><span class="sxs-lookup"><span data-stu-id="61244-236"><b>MergeFiles</b>: merges all files from hello source folder tooone file.</span></span> <span data-ttu-id="61244-237">Pokud je zadán hello název souboru nebo objekt Blob, název sloučené souboru hello by být zadaný název hello; jinak by automaticky generovaný soubor název.</span><span class="sxs-lookup"><span data-stu-id="61244-237">If hello File/Blob Name is specified, hello merged file name would be hello specified name; otherwise, would be auto-generated file name.</span></span> |<span data-ttu-id="61244-238">Ne</span><span class="sxs-lookup"><span data-stu-id="61244-238">No</span></span> |

<span data-ttu-id="61244-239">**BlobSource** také podporuje tyto dvě vlastnosti z důvodu zpětné kompatibility.</span><span class="sxs-lookup"><span data-stu-id="61244-239">**BlobSource** also supports these two properties for backward compatibility.</span></span>

* <span data-ttu-id="61244-240">**treatEmptyAsNull**: Určuje, zda tootreat hodnotu null nebo prázdný řetězec jako hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="61244-240">**treatEmptyAsNull**: Specifies whether tootreat null or empty string as null value.</span></span>
* <span data-ttu-id="61244-241">**skipHeaderLineCount** -Určuje, kolik řádků potřebovat přeskočen.</span><span class="sxs-lookup"><span data-stu-id="61244-241">**skipHeaderLineCount** - Specifies how many lines need be skipped.</span></span> <span data-ttu-id="61244-242">Vztahuje se pouze pokud vstupní datové sady používá TextFormat.</span><span class="sxs-lookup"><span data-stu-id="61244-242">It is applicable only when input dataset is using TextFormat.</span></span>

<span data-ttu-id="61244-243">Podobně **BlobSink** podporuje hello následující vlastnosti pro zpětnou kompatibilitu.</span><span class="sxs-lookup"><span data-stu-id="61244-243">Similarly, **BlobSink** supports hello following property for backward compatibility.</span></span>

* <span data-ttu-id="61244-244">**blobWriterAddHeader**: Určuje, zda tooadd hlavičku definice sloupců při zápisu tooan výstupní datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="61244-244">**blobWriterAddHeader**: Specifies whether tooadd a header of column definitions while writing tooan output dataset.</span></span>

<span data-ttu-id="61244-245">Datové sady nyní podporu hello následující vlastnosti, které implementují hello stejnou funkci: **treatEmptyAsNull**, **skipLineCount**, **firstRowAsHeader**.</span><span class="sxs-lookup"><span data-stu-id="61244-245">Datasets now support hello following properties that implement hello same functionality: **treatEmptyAsNull**, **skipLineCount**, **firstRowAsHeader**.</span></span>

<span data-ttu-id="61244-246">Hello následující tabulka obsahuje informace o použití hello nové vlastnosti datové sady místo tyto vlastnosti Zdroj/jímka objektů blob.</span><span class="sxs-lookup"><span data-stu-id="61244-246">hello following table provides guidance on using hello new dataset properties in place of these blob source/sink properties.</span></span>

| <span data-ttu-id="61244-247">Vlastnost aktivity kopírování</span><span class="sxs-lookup"><span data-stu-id="61244-247">Copy Activity property</span></span> | <span data-ttu-id="61244-248">Vlastnost DataSet</span><span class="sxs-lookup"><span data-stu-id="61244-248">Dataset property</span></span> |
|:--- |:--- |
| <span data-ttu-id="61244-249">skipHeaderLineCount na BlobSource</span><span class="sxs-lookup"><span data-stu-id="61244-249">skipHeaderLineCount on BlobSource</span></span> |<span data-ttu-id="61244-250">skipLineCount a firstRowAsHeader.</span><span class="sxs-lookup"><span data-stu-id="61244-250">skipLineCount and firstRowAsHeader.</span></span> <span data-ttu-id="61244-251">Řádky jsou přeskočeny první a pak je pro čtení hello první řádek jako záhlaví.</span><span class="sxs-lookup"><span data-stu-id="61244-251">Lines are skipped first and then hello first row is read as a header.</span></span> |
| <span data-ttu-id="61244-252">treatEmptyAsNull na BlobSource</span><span class="sxs-lookup"><span data-stu-id="61244-252">treatEmptyAsNull on BlobSource</span></span> |<span data-ttu-id="61244-253">treatEmptyAsNull na vstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="61244-253">treatEmptyAsNull on input dataset</span></span> |
| <span data-ttu-id="61244-254">blobWriterAddHeader na BlobSink</span><span class="sxs-lookup"><span data-stu-id="61244-254">blobWriterAddHeader on BlobSink</span></span> |<span data-ttu-id="61244-255">firstRowAsHeader na výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="61244-255">firstRowAsHeader on output dataset</span></span> |

<span data-ttu-id="61244-256">V tématu [zadání TextFormat](data-factory-supported-file-and-compression-formats.md#text-format) části Podrobné informace o těchto vlastností.</span><span class="sxs-lookup"><span data-stu-id="61244-256">See [Specifying TextFormat](data-factory-supported-file-and-compression-formats.md#text-format) section for detailed information on these properties.</span></span>    

### <a name="recursive-and-copybehavior-examples"></a><span data-ttu-id="61244-257">Příklady rekurzivní a copyBehavior</span><span class="sxs-lookup"><span data-stu-id="61244-257">recursive and copyBehavior examples</span></span>
<span data-ttu-id="61244-258">Tato část popisuje hello výsledné chování hello kopírování pro různé kombinace hodnot rekurzivní a copyBehavior.</span><span class="sxs-lookup"><span data-stu-id="61244-258">This section describes hello resulting behavior of hello Copy operation for different combinations of recursive and copyBehavior values.</span></span>

| <span data-ttu-id="61244-259">Rekurzivní</span><span class="sxs-lookup"><span data-stu-id="61244-259">recursive</span></span> | <span data-ttu-id="61244-260">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="61244-260">copyBehavior</span></span> | <span data-ttu-id="61244-261">Výsledné chování</span><span class="sxs-lookup"><span data-stu-id="61244-261">Resulting behavior</span></span> |
| --- | --- | --- |
| <span data-ttu-id="61244-262">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="61244-262">true</span></span> |<span data-ttu-id="61244-263">preserveHierarchy</span><span class="sxs-lookup"><span data-stu-id="61244-263">preserveHierarchy</span></span> |<span data-ttu-id="61244-264">Pro zdrojové složky složku1 s hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="61244-264">For a source folder Folder1 with hello following structure:</span></span> <br/><br/><span data-ttu-id="61244-265">Složku1</span><span class="sxs-lookup"><span data-stu-id="61244-265">Folder1</span></span><br/><span data-ttu-id="61244-266">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="61244-266">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="61244-267">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="61244-267">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="61244-268">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="61244-268">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="61244-269">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="61244-269">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="61244-270">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="61244-270">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="61244-271">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="61244-271">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="61244-272">Hello cílové složce složku1 je vytvořena s hello stejné struktury jako zdroj hello</span><span class="sxs-lookup"><span data-stu-id="61244-272">hello target folder Folder1 is created with hello same structure as hello source</span></span><br/><br/><span data-ttu-id="61244-273">Složku1</span><span class="sxs-lookup"><span data-stu-id="61244-273">Folder1</span></span><br/><span data-ttu-id="61244-274">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="61244-274">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="61244-275">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="61244-275">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="61244-276">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="61244-276">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="61244-277">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="61244-277">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="61244-278">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="61244-278">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="61244-279">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.</span><span class="sxs-lookup"><span data-stu-id="61244-279">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.</span></span> |
| <span data-ttu-id="61244-280">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="61244-280">true</span></span> |<span data-ttu-id="61244-281">flattenHierarchy</span><span class="sxs-lookup"><span data-stu-id="61244-281">flattenHierarchy</span></span> |<span data-ttu-id="61244-282">Pro zdrojové složky složku1 s hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="61244-282">For a source folder Folder1 with hello following structure:</span></span> <br/><br/><span data-ttu-id="61244-283">Složku1</span><span class="sxs-lookup"><span data-stu-id="61244-283">Folder1</span></span><br/><span data-ttu-id="61244-284">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="61244-284">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="61244-285">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="61244-285">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="61244-286">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="61244-286">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="61244-287">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="61244-287">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="61244-288">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="61244-288">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="61244-289">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="61244-289">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="61244-290">Vytvoření cíle Hello složku1 s hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="61244-290">hello target Folder1 is created with hello following structure:</span></span> <br/><br/><span data-ttu-id="61244-291">Složku1</span><span class="sxs-lookup"><span data-stu-id="61244-291">Folder1</span></span><br/><span data-ttu-id="61244-292">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File1</span><span class="sxs-lookup"><span data-stu-id="61244-292">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File1</span></span><br/><span data-ttu-id="61244-293">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File2</span><span class="sxs-lookup"><span data-stu-id="61244-293">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File2</span></span><br/><span data-ttu-id="61244-294">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název soubor3</span><span class="sxs-lookup"><span data-stu-id="61244-294">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File3</span></span><br/><span data-ttu-id="61244-295">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File4</span><span class="sxs-lookup"><span data-stu-id="61244-295">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File4</span></span><br/><span data-ttu-id="61244-296">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File5</span><span class="sxs-lookup"><span data-stu-id="61244-296">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File5</span></span> |
| <span data-ttu-id="61244-297">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="61244-297">true</span></span> |<span data-ttu-id="61244-298">mergeFiles</span><span class="sxs-lookup"><span data-stu-id="61244-298">mergeFiles</span></span> |<span data-ttu-id="61244-299">Pro zdrojové složky složku1 s hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="61244-299">For a source folder Folder1 with hello following structure:</span></span> <br/><br/><span data-ttu-id="61244-300">Složku1</span><span class="sxs-lookup"><span data-stu-id="61244-300">Folder1</span></span><br/><span data-ttu-id="61244-301">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="61244-301">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="61244-302">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="61244-302">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="61244-303">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="61244-303">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="61244-304">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="61244-304">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="61244-305">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="61244-305">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="61244-306">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="61244-306">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="61244-307">Vytvoření cíle Hello složku1 s hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="61244-307">hello target Folder1 is created with hello following structure:</span></span> <br/><br/><span data-ttu-id="61244-308">Složku1</span><span class="sxs-lookup"><span data-stu-id="61244-308">Folder1</span></span><br/><span data-ttu-id="61244-309">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + soubor3 + File4 + soubor 5 obsah jsou sloučeny do jednoho souboru s názvem automaticky generovaný soubor</span><span class="sxs-lookup"><span data-stu-id="61244-309">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + File3 + File4 + File 5 contents are merged into one file with auto-generated file name</span></span> |
| <span data-ttu-id="61244-310">False</span><span class="sxs-lookup"><span data-stu-id="61244-310">false</span></span> |<span data-ttu-id="61244-311">preserveHierarchy</span><span class="sxs-lookup"><span data-stu-id="61244-311">preserveHierarchy</span></span> |<span data-ttu-id="61244-312">Pro zdrojové složky složku1 s hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="61244-312">For a source folder Folder1 with hello following structure:</span></span> <br/><br/><span data-ttu-id="61244-313">Složku1</span><span class="sxs-lookup"><span data-stu-id="61244-313">Folder1</span></span><br/><span data-ttu-id="61244-314">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="61244-314">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="61244-315">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="61244-315">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="61244-316">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="61244-316">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="61244-317">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="61244-317">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="61244-318">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="61244-318">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="61244-319">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="61244-319">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="61244-320">Hello cílové složce složku1 je vytvořena s hello strukturu</span><span class="sxs-lookup"><span data-stu-id="61244-320">hello target folder Folder1 is created with hello following structure</span></span><br/><br/><span data-ttu-id="61244-321">Složku1</span><span class="sxs-lookup"><span data-stu-id="61244-321">Folder1</span></span><br/><span data-ttu-id="61244-322">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="61244-322">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="61244-323">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="61244-323">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><br/><br/><span data-ttu-id="61244-324">Subfolder1 s soubor3, File4 a File5 nejsou zachyceny.</span><span class="sxs-lookup"><span data-stu-id="61244-324">Subfolder1 with File3, File4, and File5 are not picked up.</span></span> |
| <span data-ttu-id="61244-325">False</span><span class="sxs-lookup"><span data-stu-id="61244-325">false</span></span> |<span data-ttu-id="61244-326">flattenHierarchy</span><span class="sxs-lookup"><span data-stu-id="61244-326">flattenHierarchy</span></span> |<span data-ttu-id="61244-327">Pro zdrojové složky složku1 s hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="61244-327">For a source folder Folder1 with hello following structure:</span></span><br/><br/><span data-ttu-id="61244-328">Složku1</span><span class="sxs-lookup"><span data-stu-id="61244-328">Folder1</span></span><br/><span data-ttu-id="61244-329">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="61244-329">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="61244-330">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="61244-330">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="61244-331">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="61244-331">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="61244-332">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="61244-332">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="61244-333">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="61244-333">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="61244-334">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="61244-334">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="61244-335">Hello cílové složce složku1 je vytvořena s hello strukturu</span><span class="sxs-lookup"><span data-stu-id="61244-335">hello target folder Folder1 is created with hello following structure</span></span><br/><br/><span data-ttu-id="61244-336">Složku1</span><span class="sxs-lookup"><span data-stu-id="61244-336">Folder1</span></span><br/><span data-ttu-id="61244-337">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File1</span><span class="sxs-lookup"><span data-stu-id="61244-337">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File1</span></span><br/><span data-ttu-id="61244-338">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File2</span><span class="sxs-lookup"><span data-stu-id="61244-338">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File2</span></span><br/><br/><br/><span data-ttu-id="61244-339">Subfolder1 s soubor3, File4 a File5 nejsou zachyceny.</span><span class="sxs-lookup"><span data-stu-id="61244-339">Subfolder1 with File3, File4, and File5 are not picked up.</span></span> |
| <span data-ttu-id="61244-340">False</span><span class="sxs-lookup"><span data-stu-id="61244-340">false</span></span> |<span data-ttu-id="61244-341">mergeFiles</span><span class="sxs-lookup"><span data-stu-id="61244-341">mergeFiles</span></span> |<span data-ttu-id="61244-342">Pro zdrojové složky složku1 s hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="61244-342">For a source folder Folder1 with hello following structure:</span></span><br/><br/><span data-ttu-id="61244-343">Složku1</span><span class="sxs-lookup"><span data-stu-id="61244-343">Folder1</span></span><br/><span data-ttu-id="61244-344">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="61244-344">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="61244-345">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="61244-345">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="61244-346">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="61244-346">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="61244-347">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="61244-347">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="61244-348">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="61244-348">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="61244-349">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="61244-349">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="61244-350">Hello cílové složce složku1 je vytvořena s hello strukturu</span><span class="sxs-lookup"><span data-stu-id="61244-350">hello target folder Folder1 is created with hello following structure</span></span><br/><br/><span data-ttu-id="61244-351">Složku1</span><span class="sxs-lookup"><span data-stu-id="61244-351">Folder1</span></span><br/><span data-ttu-id="61244-352">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 obsah jsou sloučeny do jednoho souboru s názvem automaticky generovaný soubor.</span><span class="sxs-lookup"><span data-stu-id="61244-352">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 contents are merged into one file with auto-generated file name.</span></span> <span data-ttu-id="61244-353">automaticky generovaný název File1</span><span class="sxs-lookup"><span data-stu-id="61244-353">auto-generated name for File1</span></span><br/><br/><span data-ttu-id="61244-354">Subfolder1 s soubor3, File4 a File5 nejsou zachyceny.</span><span class="sxs-lookup"><span data-stu-id="61244-354">Subfolder1 with File3, File4, and File5 are not picked up.</span></span> |

## <a name="walkthrough-use-copy-wizard-toocopy-data-tofrom-blob-storage"></a><span data-ttu-id="61244-355">Návod: Použití Průvodce kopírováním toocopy data do nebo z úložiště objektů Blob</span><span class="sxs-lookup"><span data-stu-id="61244-355">Walkthrough: Use Copy Wizard toocopy data to/from Blob Storage</span></span>
<span data-ttu-id="61244-356">Podíváme, jak tooquickly kopírování dat z Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="61244-356">Let's look at how tooquickly copy data to/from an Azure blob storage.</span></span> <span data-ttu-id="61244-357">V tomto návodu ukládá data zdrojového a cílového typu: Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="61244-357">In this walkthrough, both source and destination data stores of type: Azure Blob Storage.</span></span> <span data-ttu-id="61244-358">Hello kanál v tomto návodu kopíruje data z tooanother složka v hello stejný kontejner objektů blob.</span><span class="sxs-lookup"><span data-stu-id="61244-358">hello pipeline in this walkthrough copies data from a folder tooanother folder in hello same blob container.</span></span> <span data-ttu-id="61244-359">Tento názorný postup je záměrně jednoduchá tooshow nastavení nebo vlastnosti při používání Blob Storage jako zdroj nebo jímky.</span><span class="sxs-lookup"><span data-stu-id="61244-359">This walkthrough is intentionally simple tooshow you settings or properties when using Blob Storage as a source or sink.</span></span> 

### <a name="prerequisites"></a><span data-ttu-id="61244-360">Požadavky</span><span class="sxs-lookup"><span data-stu-id="61244-360">Prerequisites</span></span>
1. <span data-ttu-id="61244-361">Vytvoření pro obecné účely **účet úložiště Azure** Pokud již nemáte.</span><span class="sxs-lookup"><span data-stu-id="61244-361">Create a general-purpose **Azure Storage Account** if you don't have one already.</span></span> <span data-ttu-id="61244-362">Používání úložiště blob hello jako obě **zdroj** a **cílové** úložiště dat v tomto návodu.</span><span class="sxs-lookup"><span data-stu-id="61244-362">You use hello blob storage as both **source** and **destination** data store in this walkthrough.</span></span> <span data-ttu-id="61244-363">Pokud nemáte účet úložiště Azure, najdete v části hello [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) najdete v článku kroky toocreate jeden.</span><span class="sxs-lookup"><span data-stu-id="61244-363">if you don't have an Azure storage account, see hello [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) article for steps toocreate one.</span></span>
2. <span data-ttu-id="61244-364">Vytvořte kontejner objektů blob s názvem **adfblobconnector** v účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="61244-364">Create a blob container named **adfblobconnector** in hello storage account.</span></span> 
4. <span data-ttu-id="61244-365">Vytvořte složku s názvem **vstupní** v hello **adfblobconnector** kontejneru.</span><span class="sxs-lookup"><span data-stu-id="61244-365">Create a folder named **input** in hello **adfblobconnector** container.</span></span>
5. <span data-ttu-id="61244-366">Vytvořte soubor s názvem **emp.txt** s hello následující obsahu a nahrajte ho toohello **vstupní** složky pomocí nástrojů, jako například [Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/)</span><span class="sxs-lookup"><span data-stu-id="61244-366">Create a file named **emp.txt** with hello following content and upload it toohello **input** folder by using tools such as [Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/)</span></span>
    ```json
    John, Doe
    Jane, Doe
    ```
### <a name="create-hello-data-factory"></a><span data-ttu-id="61244-367">Vytvoření objektu pro vytváření dat hello</span><span class="sxs-lookup"><span data-stu-id="61244-367">Create hello data factory</span></span>
1. <span data-ttu-id="61244-368">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="61244-368">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="61244-369">Klikněte na tlačítko **+ nový** z levého horního rohu hello, klikněte na tlačítko **Intelligence + analýzy**a klikněte na tlačítko **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="61244-369">Click **+ NEW** from hello top-left corner, click **Intelligence + analytics**, and click **Data Factory**.</span></span>
3. <span data-ttu-id="61244-370">V hello **nový objekt pro vytváření dat** okno:</span><span class="sxs-lookup"><span data-stu-id="61244-370">In hello **New data factory** blade:</span></span>   
    1. <span data-ttu-id="61244-371">Zadejte **ADFBlobConnectorDF** pro hello **název**.</span><span class="sxs-lookup"><span data-stu-id="61244-371">Enter **ADFBlobConnectorDF** for hello **name**.</span></span> <span data-ttu-id="61244-372">Hello název objektu pro vytváření dat Azure hello musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="61244-372">hello name of hello Azure data factory must be globally unique.</span></span> <span data-ttu-id="61244-373">Pokud se zobrazí chyba hello: `*Data factory name “ADFBlobConnectorDF” is not available`, změňte hello název objektu pro vytváření dat hello (například yournameADFBlobConnectorDF) a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="61244-373">If you receive hello error: `*Data factory name “ADFBlobConnectorDF” is not available`, change hello name of hello data factory (for example, yournameADFBlobConnectorDF) and try creating again.</span></span> <span data-ttu-id="61244-374">V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="61244-374">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>
    2. <span data-ttu-id="61244-375">Vyberte své **předplatné** Azure.</span><span class="sxs-lookup"><span data-stu-id="61244-375">Select your Azure **subscription**.</span></span>
    3. <span data-ttu-id="61244-376">Pro skupinu prostředků, vyberte **použít existující** tooselect stávající prostředek skupiny (nebo) vyberte **vytvořit nový** tooenter název pro skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="61244-376">For Resource Group, select **Use existing** tooselect an existing resource group (or) select **Create new** tooenter a name for a resource group.</span></span>
    4. <span data-ttu-id="61244-377">Vyberte **umístění** hello data Factory.</span><span class="sxs-lookup"><span data-stu-id="61244-377">Select a **location** for hello data factory.</span></span>
    5. <span data-ttu-id="61244-378">Vyberte **Pin toodashboard** políčko v hello dolní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="61244-378">Select **Pin toodashboard** check box at hello bottom of hello blade.</span></span>
    6. <span data-ttu-id="61244-379">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="61244-379">Click **Create**.</span></span>
3. <span data-ttu-id="61244-380">Po dokončení vytvoření hello uvidíte hello **Data Factory** jak je uvedeno v následující obrázek hello: ![Domovská stránka objektu pro vytváření dat](./media/data-factory-azure-blob-connector/data-factory-home-page.png)</span><span class="sxs-lookup"><span data-stu-id="61244-380">After hello creation is complete, you see hello **Data Factory** blade as shown in hello following image: ![Data factory home page](./media/data-factory-azure-blob-connector/data-factory-home-page.png)</span></span>

### <a name="copy-wizard"></a><span data-ttu-id="61244-381">Průvodce kopírováním</span><span class="sxs-lookup"><span data-stu-id="61244-381">Copy Wizard</span></span>
1. <span data-ttu-id="61244-382">Na domovské stránce objektu pro vytváření dat hello, klikněte na tlačítko hello **kopírování dat [PREVIEW]** dlaždici toolaunch **Průvodce kopírováním dat** na samostatné kartě.</span><span class="sxs-lookup"><span data-stu-id="61244-382">On hello Data Factory home page, click hello **Copy data [PREVIEW]** tile toolaunch **Copy Data Wizard** in a separate tab.</span></span>    
    
    > [!NOTE]
    >    <span data-ttu-id="61244-383">Pokud se zobrazí, že hello webový prohlížeč zasekl ve fázi "autorizace …", zakažte/zrušte zaškrtnutí políčka **blokovat soubory cookie třetích stran a data lokality** nastavení (nebo) zachovat povolené a vytvořte výjimku pro **login.microsoftonline.com**a poté se pokuste spustit hello průvodce znovu.</span><span class="sxs-lookup"><span data-stu-id="61244-383">If you see that hello web browser is stuck at "Authorizing...", disable/uncheck **Block third-party cookies and site data** setting (or) keep it enabled and create an exception for **login.microsoftonline.com** and then try launching hello wizard again.</span></span>
2. <span data-ttu-id="61244-384">V hello **vlastnosti** stránky:</span><span class="sxs-lookup"><span data-stu-id="61244-384">In hello **Properties** page:</span></span>
    1. <span data-ttu-id="61244-385">Zadejte **CopyPipeline** pro **název úlohy**.</span><span class="sxs-lookup"><span data-stu-id="61244-385">Enter **CopyPipeline** for **Task name**.</span></span> <span data-ttu-id="61244-386">Název úlohy Hello je název hello hello kanálu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="61244-386">hello task name is hello name of hello pipeline in your data factory.</span></span>
    2. <span data-ttu-id="61244-387">Zadejte **popis** pro úlohu hello (volitelné).</span><span class="sxs-lookup"><span data-stu-id="61244-387">Enter a **description** for hello task (optional).</span></span>
    3. <span data-ttu-id="61244-388">Pro **cadence úloh nebo plán úloh**, zachovat hello **spuštění pravidelně podle plánu** možnost.</span><span class="sxs-lookup"><span data-stu-id="61244-388">For **Task cadence or Task schedule**, keep hello **Run regularly on schedule** option.</span></span> <span data-ttu-id="61244-389">Pokud chcete toorun tato úloha pouze jednou místo opakovaně spouštět podle plánu, vyberte **spustit jednou nyní**.</span><span class="sxs-lookup"><span data-stu-id="61244-389">If you want toorun this task only once instead of run repeatedly on a schedule, select **Run once now**.</span></span> <span data-ttu-id="61244-390">Pokud vyberete, **spustit jednou nyní** možnost, [jednorázového kanálu](data-factory-create-pipelines.md#onetime-pipeline) je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="61244-390">If you select, **Run once now** option, a [one-time pipeline](data-factory-create-pipelines.md#onetime-pipeline) is created.</span></span> 
    4. <span data-ttu-id="61244-391">Zachovat nastavení hello **opakovaná vzor**.</span><span class="sxs-lookup"><span data-stu-id="61244-391">Keep hello settings for **Recurring pattern**.</span></span> <span data-ttu-id="61244-392">Tato úloha spustí každý den mezi hello začínat a končit doby můžete zadat v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="61244-392">This task runs daily between hello start and end times you specify in hello next step.</span></span>
    5. <span data-ttu-id="61244-393">Změna hello **datum a čas zahájení** příliš**21/04/2017**.</span><span class="sxs-lookup"><span data-stu-id="61244-393">Change hello **Start date time** too**04/21/2017**.</span></span> 
    6. <span data-ttu-id="61244-394">Změna hello **datum a čas ukončení** příliš**04/25/2017**.</span><span class="sxs-lookup"><span data-stu-id="61244-394">Change hello **End date time** too**04/25/2017**.</span></span> <span data-ttu-id="61244-395">Můžete chtít tootype hello datum místo projdete hello kalendáře.</span><span class="sxs-lookup"><span data-stu-id="61244-395">You may want tootype hello date instead of browsing through hello calendar.</span></span>     
    8. <span data-ttu-id="61244-396">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="61244-396">Click **Next**.</span></span>
      <span data-ttu-id="61244-397">![Nástroj pro kopírování – stránka vlastností](./media/data-factory-azure-blob-connector/copy-tool-properties-page.png)</span><span class="sxs-lookup"><span data-stu-id="61244-397">![Copy Tool - Properties page](./media/data-factory-azure-blob-connector/copy-tool-properties-page.png)</span></span> 
3. <span data-ttu-id="61244-398">Na hello **zdrojového úložiště dat** klikněte na tlačítko **Azure Blob Storage** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="61244-398">On hello **Source data store** page, click **Azure Blob Storage** tile.</span></span> <span data-ttu-id="61244-399">Tato stránka toospecify hello zdrojového úložiště dat se používá pro úlohy kopie hello.</span><span class="sxs-lookup"><span data-stu-id="61244-399">You use this page toospecify hello source data store for hello copy task.</span></span> <span data-ttu-id="61244-400">Můžete použít existující propojenou službu úložiště dat nebo zadat nové úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="61244-400">You can use an existing data store linked service (or) specify a new data store.</span></span> <span data-ttu-id="61244-401">toouse existující propojená služba, byste měli vybrat **z existujících PROPOJENÝCH služeb** a hello vyberte požadovanou propojenou službu.</span><span class="sxs-lookup"><span data-stu-id="61244-401">toouse an existing linked service, you would select **FROM EXISTING LINKED SERVICES** and select hello right linked service.</span></span> 
    <span data-ttu-id="61244-402">![Nástroj pro kopírování – stránka zdrojového úložiště dat](./media/data-factory-azure-blob-connector/copy-tool-source-data-store-page.png)</span><span class="sxs-lookup"><span data-stu-id="61244-402">![Copy Tool - Source data store page](./media/data-factory-azure-blob-connector/copy-tool-source-data-store-page.png)</span></span>
4. <span data-ttu-id="61244-403">Na hello **zadejte účet úložiště Azure Blob hello** stránky:</span><span class="sxs-lookup"><span data-stu-id="61244-403">On hello **Specify hello Azure Blob storage account** page:</span></span>
   1. <span data-ttu-id="61244-404">Zachovat hello automaticky generovaný název pro **název připojení**.</span><span class="sxs-lookup"><span data-stu-id="61244-404">Keep hello auto-generated name for **Connection name**.</span></span> <span data-ttu-id="61244-405">Název připojení Hello je název hello hello propojené služby typu: Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="61244-405">hello connection name is hello name of hello linked service of type: Azure Storage.</span></span> 
   2. <span data-ttu-id="61244-406">Ujistěte se, že je pro položku **Metoda výběru účtu** vybrána možnost **Z předplatných Azure**.</span><span class="sxs-lookup"><span data-stu-id="61244-406">Confirm that **From Azure subscriptions** option is selected for **Account selection method**.</span></span>
   3. <span data-ttu-id="61244-407">Vyberte předplatné Azure nebo ponechte **Vybrat vše** pro **předplatné**.</span><span class="sxs-lookup"><span data-stu-id="61244-407">Select your Azure subscription or keep **Select all** for **Azure subscription**.</span></span>   
   4. <span data-ttu-id="61244-408">Vyberte **účtu úložiště Azure** z hello účty k dispozici v rámci předplatného hello vybraný seznam úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="61244-408">Select an **Azure storage account** from hello list of Azure storage accounts available in hello selected subscription.</span></span> <span data-ttu-id="61244-409">Můžete také tooenter nastavení účtu úložiště ručně tak, že vyberete **zadat ručně** možnost pro hello **účet metodu výběru**.</span><span class="sxs-lookup"><span data-stu-id="61244-409">You can also choose tooenter storage account settings manually by selecting **Enter manually** option for hello **Account selection method**.</span></span>
   5. <span data-ttu-id="61244-410">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="61244-410">Click **Next**.</span></span> 
      <span data-ttu-id="61244-411">![Nástroj pro kopírování – zadání účtu Azure Blob storage hello](./media/data-factory-azure-blob-connector/copy-tool-specify-azure-blob-storage-account.png)</span><span class="sxs-lookup"><span data-stu-id="61244-411">![Copy Tool - Specify hello Azure Blob storage account](./media/data-factory-azure-blob-connector/copy-tool-specify-azure-blob-storage-account.png)</span></span>
5. <span data-ttu-id="61244-412">Na **zvolte hello vstupního souboru nebo složky** stránky:</span><span class="sxs-lookup"><span data-stu-id="61244-412">On **Choose hello input file or folder** page:</span></span>
   1. <span data-ttu-id="61244-413">Klikněte dvakrát na **adfblobcontainer**.</span><span class="sxs-lookup"><span data-stu-id="61244-413">Double-click **adfblobcontainer**.</span></span>
   2. <span data-ttu-id="61244-414">Vyberte **vstupní**a klikněte na tlačítko **zvolte**.</span><span class="sxs-lookup"><span data-stu-id="61244-414">Select **input**, and click **Choose**.</span></span> <span data-ttu-id="61244-415">V tomto návodu vyberte vstupní složky hello.</span><span class="sxs-lookup"><span data-stu-id="61244-415">In this walkthrough, you select hello input folder.</span></span> <span data-ttu-id="61244-416">Můžete třeba také vybrat hello emp.txt soubor ve složce hello místo.</span><span class="sxs-lookup"><span data-stu-id="61244-416">You could also select hello emp.txt file in hello folder instead.</span></span> 
      <span data-ttu-id="61244-417">![Nástroj pro kopírování – volba hello vstupního souboru nebo složky](./media/data-factory-azure-blob-connector/copy-tool-choose-input-file-or-folder.png)</span><span class="sxs-lookup"><span data-stu-id="61244-417">![Copy Tool - Choose hello input file or folder](./media/data-factory-azure-blob-connector/copy-tool-choose-input-file-or-folder.png)</span></span>
6. <span data-ttu-id="61244-418">Na hello **zvolte hello vstupního souboru nebo složky** stránky:</span><span class="sxs-lookup"><span data-stu-id="61244-418">On hello **Choose hello input file or folder** page:</span></span>
    1. <span data-ttu-id="61244-419">Potvrďte, že hello **souboru nebo složky** je nastaven příliš**adfblobconnector/vstup**.</span><span class="sxs-lookup"><span data-stu-id="61244-419">Confirm that hello **file or folder** is set too**adfblobconnector/input**.</span></span> <span data-ttu-id="61244-420">Pokud jsou soubory hello do podsložek, například 2017/04/01, 2017/04/02 a tak dále, zadání adfblobconnector / / {year} / {month} / {day} pro soubor nebo složku.</span><span class="sxs-lookup"><span data-stu-id="61244-420">If hello files are in sub folders, for example, 2017/04/01, 2017/04/02, and so on, enter adfblobconnector/input/{year}/{month}/{day} for file or folder.</span></span> <span data-ttu-id="61244-421">Po stisknutí klávesy TAB mimo hello textového pole, uvidíte tři formáty tooselect rozevírací seznamy (rrrr) rok, měsíc (MM) a den (dd).</span><span class="sxs-lookup"><span data-stu-id="61244-421">When you press TAB out of hello text box, you see three drop-down lists tooselect formats for year (yyyy), month (MM), and day (dd).</span></span> 
    2. <span data-ttu-id="61244-422">Nenastavujte **zkopírujte soubor rekurzivně**.</span><span class="sxs-lookup"><span data-stu-id="61244-422">Do not set **Copy file recursively**.</span></span> <span data-ttu-id="61244-423">Vyberte tuto možnost toorecursively křížovou prostřednictvím složek pro soubory toobe zkopírovaný toohello cíl.</span><span class="sxs-lookup"><span data-stu-id="61244-423">Select this option toorecursively traverse through folders for files toobe copied toohello destination.</span></span> 
    3. <span data-ttu-id="61244-424">Není hello **binární kopie** možnost.</span><span class="sxs-lookup"><span data-stu-id="61244-424">Do not hello **binary copy** option.</span></span> <span data-ttu-id="61244-425">Vyberte tuto možnost tooperform binární kopie cíl toohello zdrojového souboru.</span><span class="sxs-lookup"><span data-stu-id="61244-425">Select this option tooperform a binary copy of source file toohello destination.</span></span> <span data-ttu-id="61244-426">Nevybírejte v tomto návodu tak, aby se zobrazí další možnosti v dalších stránkách hello.</span><span class="sxs-lookup"><span data-stu-id="61244-426">Do not select for this walkthrough so that you can see more options in hello next pages.</span></span> 
    4. <span data-ttu-id="61244-427">Potvrďte, že hello **typ komprese** je nastaven příliš**žádné**.</span><span class="sxs-lookup"><span data-stu-id="61244-427">Confirm that hello **Compression type** is set too**None**.</span></span> <span data-ttu-id="61244-428">Hodnotu pro tuto možnost vyberte, pokud jsou v jednom z formátů hello podporované komprimované zdrojové soubory.</span><span class="sxs-lookup"><span data-stu-id="61244-428">Select a value for this option if your source files are compressed in one of hello supported formats.</span></span> 
    5. <span data-ttu-id="61244-429">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="61244-429">Click **Next**.</span></span>
    <span data-ttu-id="61244-430">![Nástroj pro kopírování – volba hello vstupního souboru nebo složky](./media/data-factory-azure-blob-connector/chose-input-file-folder.png)</span><span class="sxs-lookup"><span data-stu-id="61244-430">![Copy Tool - Choose hello input file or folder](./media/data-factory-azure-blob-connector/chose-input-file-folder.png)</span></span> 
7. <span data-ttu-id="61244-431">Na hello **nastavení formátu souboru** stránky, zobrazí hello oddělovače a hello schématu, která je automaticky nalezeny průvodcem hello analýzou soubor hello.</span><span class="sxs-lookup"><span data-stu-id="61244-431">On hello **File format settings** page, you see hello delimiters and hello schema that is auto-detected by hello wizard by parsing hello file.</span></span> 
    1. <span data-ttu-id="61244-432">Potvrďte hello následující možnosti:.</span><span class="sxs-lookup"><span data-stu-id="61244-432">Confirm hello following options: a.</span></span> <span data-ttu-id="61244-433">Hello **formát souboru** je nastaven příliš**formátu textu**.</span><span class="sxs-lookup"><span data-stu-id="61244-433">hello **file format** is set too**Text format**.</span></span> <span data-ttu-id="61244-434">Můžete zobrazit všechny formáty hello podporované v rozevíracím seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="61244-434">You can see all hello supported formats in hello drop-down list.</span></span> <span data-ttu-id="61244-435">Příklad: formát JSON, Avro, ORC, Parquet.</span><span class="sxs-lookup"><span data-stu-id="61244-435">For example: JSON, Avro, ORC, Parquet.</span></span>
        <span data-ttu-id="61244-436">b.</span><span class="sxs-lookup"><span data-stu-id="61244-436">b.</span></span> <span data-ttu-id="61244-437">Hello **sloupec oddělovač** je nastaven příliš`Comma (,)`.</span><span class="sxs-lookup"><span data-stu-id="61244-437">hello **column delimiter** is set too`Comma (,)`.</span></span> <span data-ttu-id="61244-438">Zobrazí se text hello další sloupec oddělovače v rozevíracím seznamu hello podporovány službou Data Factory.</span><span class="sxs-lookup"><span data-stu-id="61244-438">You can see hello other column delimiters supported by Data Factory in hello drop-down list.</span></span> <span data-ttu-id="61244-439">Můžete také zadat vlastní oddělovač.</span><span class="sxs-lookup"><span data-stu-id="61244-439">You can also specify a custom delimiter.</span></span>
        <span data-ttu-id="61244-440">c.</span><span class="sxs-lookup"><span data-stu-id="61244-440">c.</span></span> <span data-ttu-id="61244-441">Hello **oddělovač řádků** je nastaven příliš`Carriage Return + Line feed (\r\n)`.</span><span class="sxs-lookup"><span data-stu-id="61244-441">hello **row delimiter** is set too`Carriage Return + Line feed (\r\n)`.</span></span> <span data-ttu-id="61244-442">Zobrazí se další řádek oddělovače podporovaných službou Data Factory v rozevíracím seznamu hello hello.</span><span class="sxs-lookup"><span data-stu-id="61244-442">You can see hello other row delimiters supported by Data Factory in hello drop-down list.</span></span> <span data-ttu-id="61244-443">Můžete také zadat vlastní oddělovač.</span><span class="sxs-lookup"><span data-stu-id="61244-443">You can also specify a custom delimiter.</span></span>
        <span data-ttu-id="61244-444">d.</span><span class="sxs-lookup"><span data-stu-id="61244-444">d.</span></span> <span data-ttu-id="61244-445">Hello **Přeskočit počet řádků** je nastaven příliš**0**.</span><span class="sxs-lookup"><span data-stu-id="61244-445">hello **skip line count** is set too**0**.</span></span> <span data-ttu-id="61244-446">Pokud chcete, aby přeskočil hello horní části souboru hello pár řádků toobe, zadejte zde číslo hello.</span><span class="sxs-lookup"><span data-stu-id="61244-446">If you want a few lines toobe skipped at hello top of hello file, enter hello number here.</span></span>
        <span data-ttu-id="61244-447">e.</span><span class="sxs-lookup"><span data-stu-id="61244-447">e.</span></span>  <span data-ttu-id="61244-448">Hello **první řádek dat obsahuje názvy sloupců** není nastaven.</span><span class="sxs-lookup"><span data-stu-id="61244-448">hello **first data row contains column names** is not set.</span></span> <span data-ttu-id="61244-449">Pokud hello zdrojové soubory obsahují názvy sloupců v hello první řádek, vyberte tuto možnost.</span><span class="sxs-lookup"><span data-stu-id="61244-449">If hello source files contain column names in hello first row, select this option.</span></span>
        <span data-ttu-id="61244-450">f.</span><span class="sxs-lookup"><span data-stu-id="61244-450">f.</span></span> <span data-ttu-id="61244-451">Hello **považovat prázdný sloupec hodnota null** je možnost nastavena.</span><span class="sxs-lookup"><span data-stu-id="61244-451">hello **treat empty column value as null** option is set.</span></span>
    2. <span data-ttu-id="61244-452">Rozbalte položku **upřesňující nastavení** toosee rozšířené možnosti, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="61244-452">Expand **Advanced settings** toosee advanced option available.</span></span>
    3. <span data-ttu-id="61244-453">V dolní části hello hello stránky, najdete v části hello **preview** dat ze souboru emp.txt hello.</span><span class="sxs-lookup"><span data-stu-id="61244-453">At hello bottom of hello page, see hello **preview** of data from hello emp.txt file.</span></span>
    4. <span data-ttu-id="61244-454">Klikněte na tlačítko **schématu** kartě v hello dolní toosee hello schématu tohoto Průvodce kopírováním hello odvodit prohlížením hello data hello zdrojového souboru.</span><span class="sxs-lookup"><span data-stu-id="61244-454">Click **SCHEMA** tab at hello bottom toosee hello schema that hello copy wizard inferred by looking at hello data in hello source file.</span></span>
    5. <span data-ttu-id="61244-455">Klikněte na tlačítko **Další** po zkontrolujte hello oddělovače a zobrazte náhled dat.</span><span class="sxs-lookup"><span data-stu-id="61244-455">Click **Next** after you review hello delimiters and preview data.</span></span>
    <span data-ttu-id="61244-456">![Nástroj pro kopírování – nastavení formátu souboru](./media/data-factory-azure-blob-connector/copy-tool-file-format-settings.png)</span><span class="sxs-lookup"><span data-stu-id="61244-456">![Copy Tool - File format settings](./media/data-factory-azure-blob-connector/copy-tool-file-format-settings.png)</span></span>  
8. <span data-ttu-id="61244-457">Na hello **úložiště dat cílové stránky**, vyberte **Azure Blob Storage**a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="61244-457">On hello **Destination data store page**, select **Azure Blob Storage**, and click **Next**.</span></span> <span data-ttu-id="61244-458">Hello Azure Blob Storage používají jako obou hello zdrojového a cílového úložiště dat v tomto návodu.</span><span class="sxs-lookup"><span data-stu-id="61244-458">You are using hello Azure Blob Storage as both hello source and destination data stores in this walkthrough.</span></span>    
    <span data-ttu-id="61244-459">![Nástroj pro kopírování – vyberte cílového úložiště dat](media/data-factory-azure-blob-connector/select-destination-data-store.png)</span><span class="sxs-lookup"><span data-stu-id="61244-459">![Copy Tool - select destination data store](media/data-factory-azure-blob-connector/select-destination-data-store.png)</span></span>
9. <span data-ttu-id="61244-460">Na **zadejte účet úložiště Azure Blob hello** stránky:</span><span class="sxs-lookup"><span data-stu-id="61244-460">On **Specify hello Azure Blob storage account** page:</span></span>
   1. <span data-ttu-id="61244-461">Zadejte **AzureStorageLinkedService** pro hello **název připojení** pole.</span><span class="sxs-lookup"><span data-stu-id="61244-461">Enter **AzureStorageLinkedService** for hello **Connection name** field.</span></span>
   2. <span data-ttu-id="61244-462">Ujistěte se, že je pro položku **Metoda výběru účtu** vybrána možnost **Z předplatných Azure**.</span><span class="sxs-lookup"><span data-stu-id="61244-462">Confirm that **From Azure subscriptions** option is selected for **Account selection method**.</span></span>
   3. <span data-ttu-id="61244-463">Vyberte své **předplatné** Azure.</span><span class="sxs-lookup"><span data-stu-id="61244-463">Select your Azure **subscription**.</span></span>  
   4. <span data-ttu-id="61244-464">Vyberte účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="61244-464">Select your Azure storage account.</span></span> 
   5. <span data-ttu-id="61244-465">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="61244-465">Click **Next**.</span></span>     
10. <span data-ttu-id="61244-466">Na hello **zvolte hello výstupní soubor nebo složku** stránky:</span><span class="sxs-lookup"><span data-stu-id="61244-466">On hello **Choose hello output file or folder** page:</span></span> 
    6. <span data-ttu-id="61244-467">Zadejte **cesta ke složce** jako **adfblobconnector výstupní / {year} / {month} / {day}**.</span><span class="sxs-lookup"><span data-stu-id="61244-467">specify **Folder path** as **adfblobconnector/output/{year}/{month}/{day}**.</span></span> <span data-ttu-id="61244-468">Zadejte **KARTĚ**.</span><span class="sxs-lookup"><span data-stu-id="61244-468">Enter **TAB**.</span></span>
    7. <span data-ttu-id="61244-469">Pro hello **roku**, vyberte **rrrr**.</span><span class="sxs-lookup"><span data-stu-id="61244-469">For hello **year**, select **yyyy**.</span></span>
    8. <span data-ttu-id="61244-470">Pro hello **měsíc**, potvrďte, že je nastaven příliš**MM**.</span><span class="sxs-lookup"><span data-stu-id="61244-470">For hello **month**, confirm that it is set too**MM**.</span></span>
    9. <span data-ttu-id="61244-471">Pro hello **den**, potvrďte, že je nastaven příliš**dd**.</span><span class="sxs-lookup"><span data-stu-id="61244-471">For hello **day**, confirm that it is set too**dd**.</span></span>
    10. <span data-ttu-id="61244-472">Potvrďte, že hello **typ komprese** je nastaven příliš**žádné**.</span><span class="sxs-lookup"><span data-stu-id="61244-472">Confirm that hello **compression type** is set too**None**.</span></span>
    11. <span data-ttu-id="61244-473">Potvrďte, že hello **zkopírujte chování** je nastaven příliš**sloučení souborů**.</span><span class="sxs-lookup"><span data-stu-id="61244-473">Confirm that hello **copy behavior** is set too**Merge files**.</span></span> <span data-ttu-id="61244-474">Pokud hello výstupní soubor s hello, které se stejným názvem již existuje, hello nový obsah je přidaný toohello stejný soubor na konci hello.</span><span class="sxs-lookup"><span data-stu-id="61244-474">If hello output file with hello same name already exists, hello new content is added toohello same file at hello end.</span></span>
    12. <span data-ttu-id="61244-475">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="61244-475">Click **Next**.</span></span>
    <span data-ttu-id="61244-476">![Nástroj pro kopírování – volba výstupního souboru nebo složky](media/data-factory-azure-blob-connector/choose-the-output-file-or-folder.png)</span><span class="sxs-lookup"><span data-stu-id="61244-476">![Copy Tool - Choose output file or folder](media/data-factory-azure-blob-connector/choose-the-output-file-or-folder.png)</span></span>
11. <span data-ttu-id="61244-477">Na hello **nastavení formátu souboru** stránka, zkontrolujte hello nastavení a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="61244-477">On hello **File format settings** page, review hello settings, and click **Next**.</span></span> <span data-ttu-id="61244-478">Jedním z hello zde další možnosti je tooadd soubor hlaviček toohello výstup.</span><span class="sxs-lookup"><span data-stu-id="61244-478">One of hello additional options here is tooadd a header toohello output file.</span></span> <span data-ttu-id="61244-479">Pokud tuto možnost vyberete, se přidá řádek záhlaví s názvy sloupců hello ze schématu hello hello zdroje.</span><span class="sxs-lookup"><span data-stu-id="61244-479">If you select that option, a header row is added with names of hello columns from hello schema of hello source.</span></span> <span data-ttu-id="61244-480">Při zobrazení hello schématu zdroje hello můžete přejmenovat hello výchozí názvy sloupců.</span><span class="sxs-lookup"><span data-stu-id="61244-480">You can rename hello default column names when viewing hello schema for hello source.</span></span> <span data-ttu-id="61244-481">Můžete například změnit hello první sloupec tooFirst název a druhý sloupec tooLast název.</span><span class="sxs-lookup"><span data-stu-id="61244-481">For example, you could change hello first column tooFirst Name and second column tooLast Name.</span></span> <span data-ttu-id="61244-482">Potom hello výstupní soubor je vytvořen s záhlaví s těmito názvy jako názvy sloupců.</span><span class="sxs-lookup"><span data-stu-id="61244-482">Then, hello output file is generated with a header with these names as column names.</span></span> 
    <span data-ttu-id="61244-483">![Nástroj pro kopírování – nastavení formátu souboru pro cíl](media/data-factory-azure-blob-connector/file-format-destination.png)</span><span class="sxs-lookup"><span data-stu-id="61244-483">![Copy Tool - File format settings for destination](media/data-factory-azure-blob-connector/file-format-destination.png)</span></span>
12. <span data-ttu-id="61244-484">Na hello **nastavení výkonu** potvrďte, že **cloudu jednotky** a **paralelní kopie** nastaveny příliš**automaticky**a klikněte na tlačítko Další.</span><span class="sxs-lookup"><span data-stu-id="61244-484">On hello **Performance settings** page, confirm that **cloud units** and **parallel copies** are set too**Auto**, and click Next.</span></span> <span data-ttu-id="61244-485">Podrobnosti o těchto nastaveních najdete v tématu [zkopírujte aktivity výkonu a vyladění průvodce](data-factory-copy-activity-performance.md#parallel-copy).</span><span class="sxs-lookup"><span data-stu-id="61244-485">For details about these settings, see [Copy activity performance and tuning guide](data-factory-copy-activity-performance.md#parallel-copy).</span></span>
    <span data-ttu-id="61244-486">![Nástroj pro kopírování – nastavení výkonu](media/data-factory-azure-blob-connector/copy-performance-settings.png)</span><span class="sxs-lookup"><span data-stu-id="61244-486">![Copy Tool - Performance settings](media/data-factory-azure-blob-connector/copy-performance-settings.png)</span></span> 
14. <span data-ttu-id="61244-487">Na hello **Souhrn** zkontrolujte všechna nastavení (Vlastnosti úlohy, nastavení pro zdrojové a cílové a Kopírovat nastavení) a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="61244-487">On hello **Summary** page, review all settings (task properties, settings for source and destination, and copy settings), and click **Next**.</span></span>
    <span data-ttu-id="61244-488">![Nástroj pro kopírování – stránka souhrnu](media/data-factory-azure-blob-connector/copy-tool-summary-page.png)</span><span class="sxs-lookup"><span data-stu-id="61244-488">![Copy Tool - Summary page](media/data-factory-azure-blob-connector/copy-tool-summary-page.png)</span></span>
15. <span data-ttu-id="61244-489">Zkontrolujte informace v hello **Souhrn** a klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="61244-489">Review information in hello **Summary** page, and click **Finish**.</span></span> <span data-ttu-id="61244-490">Hello Průvodce vytvoří dvě propojené služby, dvě datové sady (vstupní a výstupní) a jeden kanál v objektu pro vytváření dat hello (z kde spouštěna hello Průvodce kopírováním).</span><span class="sxs-lookup"><span data-stu-id="61244-490">hello wizard creates two linked services, two datasets (input and output), and one pipeline in hello data factory (from where you launched hello Copy Wizard).</span></span>
    <span data-ttu-id="61244-491">![Nástroj pro kopírování – stránka nasazení](media/data-factory-azure-blob-connector/copy-tool-deployment-page.png)</span><span class="sxs-lookup"><span data-stu-id="61244-491">![Copy Tool - Deployment page](media/data-factory-azure-blob-connector/copy-tool-deployment-page.png)</span></span>

### <a name="monitor-hello-pipeline-copy-task"></a><span data-ttu-id="61244-492">Monitorování kanálu hello (úlohy kopie)</span><span class="sxs-lookup"><span data-stu-id="61244-492">Monitor hello pipeline (copy task)</span></span>

1. <span data-ttu-id="61244-493">Kliknutím na odkaz hello `Click here toomonitor copy pipeline` na hello **nasazení** stránky.</span><span class="sxs-lookup"><span data-stu-id="61244-493">Click hello link `Click here toomonitor copy pipeline` on hello **Deployment** page.</span></span> 
2. <span data-ttu-id="61244-494">Měli byste vidět hello **sledovat a spravovat aplikace** na samostatné kartě.  ![Sledování a správě aplikací](media/data-factory-azure-blob-connector/monitor-manage-app.png)</span><span class="sxs-lookup"><span data-stu-id="61244-494">You should see hello **Monitor and Manage application** in a separate tab.  ![Monitor and Manage App](media/data-factory-azure-blob-connector/monitor-manage-app.png)</span></span>
3. <span data-ttu-id="61244-495">Změna hello **spustit** čas v horní části hello příliš`04/19/2017` a **end** čas příliš`04/27/2017`a potom klikněte na **použít**.</span><span class="sxs-lookup"><span data-stu-id="61244-495">Change hello **start** time at hello top too`04/19/2017` and **end** time too`04/27/2017`, and then click **Apply**.</span></span> 
4. <span data-ttu-id="61244-496">Měli byste vidět pět oken aktivity v hello **aktivity WINDOWS** seznamu.</span><span class="sxs-lookup"><span data-stu-id="61244-496">You should see five activity windows in hello **ACTIVITY WINDOWS** list.</span></span> <span data-ttu-id="61244-497">Hello **WindowStart** časy by mělo zahrnovat všechny dny z kanálu počáteční toopipeline koncový čas.</span><span class="sxs-lookup"><span data-stu-id="61244-497">hello **WindowStart** times should cover all days from pipeline start toopipeline end times.</span></span> 
5. <span data-ttu-id="61244-498">Klikněte na tlačítko **aktualizovat** tlačítko hello **aktivity WINDOWS** seznamu několikrát, dokud neuvidíte hello stav všechny aktivity windows hello je nastaven tooReady.</span><span class="sxs-lookup"><span data-stu-id="61244-498">Click **Refresh** button for hello **ACTIVITY WINDOWS** list a few times until you see hello status of all hello activity windows is set tooReady.</span></span> 
6. <span data-ttu-id="61244-499">Nyní ověřte, že jsou generovány hello výstupní soubory ve složce výstup hello adfblobconnector kontejneru.</span><span class="sxs-lookup"><span data-stu-id="61244-499">Now, verify that hello output files are generated in hello output folder of adfblobconnector container.</span></span> <span data-ttu-id="61244-500">Měli byste vidět hello následující strukturu složek v hello výstupní složky:</span><span class="sxs-lookup"><span data-stu-id="61244-500">You should see hello following folder structure in hello output folder:</span></span> 
    ```
    2017/04/21
    2017/04/22
    2017/04/23
    2017/04/24
    2017/04/25    
    ```
<span data-ttu-id="61244-501">Podrobné informace o monitorování a Správa objektů pro vytváření dat najdete v tématu [monitorování a Správa kanálů služby Data Factory](data-factory-monitor-manage-app.md) článku.</span><span class="sxs-lookup"><span data-stu-id="61244-501">For detailed information about monitoring and managing data factories, see [Monitor and manage Data Factory pipeline](data-factory-monitor-manage-app.md) article.</span></span> 
 
### <a name="data-factory-entities"></a><span data-ttu-id="61244-502">Entity objektu pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="61244-502">Data Factory entities</span></span>
<span data-ttu-id="61244-503">Nyní přejděte zpět toohello karta s domovskou stránku hello Data Factory.</span><span class="sxs-lookup"><span data-stu-id="61244-503">Now, switch back toohello tab with hello Data Factory home page.</span></span> <span data-ttu-id="61244-504">Všimněte si, že existují dvě propojené služby, dvě datové sady a jeden kanál v datové továrně teď.</span><span class="sxs-lookup"><span data-stu-id="61244-504">Notice that there are two linked services, two datasets, and one pipeline in your data factory now.</span></span> 

![Domovská stránka objektu pro vytváření dat s entitami](media/data-factory-azure-blob-connector/data-factory-home-page-with-numbers.png)

<span data-ttu-id="61244-506">Klikněte na tlačítko **vytvořit a nasadit** toolaunch editoru služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="61244-506">Click **Author and deploy** toolaunch Data Factory Editor.</span></span> 

![Data Factory Editor](media/data-factory-azure-blob-connector/data-factory-editor.png)

<span data-ttu-id="61244-508">Měli byste vidět následující entity služby Data Factory v datové továrně hello:</span><span class="sxs-lookup"><span data-stu-id="61244-508">You should see hello following Data Factory entities in your data factory:</span></span> 

 - <span data-ttu-id="61244-509">Dvě propojené služby.</span><span class="sxs-lookup"><span data-stu-id="61244-509">Two linked services.</span></span> <span data-ttu-id="61244-510">Jednu pro hello zdroje a hello jiného pro cíl hello.</span><span class="sxs-lookup"><span data-stu-id="61244-510">One for hello source and hello other one for hello destination.</span></span> <span data-ttu-id="61244-511">Obě hello propojené služby odkazovat toohello stejný účet služby Azure Storage v tomto návodu.</span><span class="sxs-lookup"><span data-stu-id="61244-511">Both hello linked services refer toohello same Azure Storage account in this walkthrough.</span></span> 
 - <span data-ttu-id="61244-512">Dvě datové sady.</span><span class="sxs-lookup"><span data-stu-id="61244-512">Two datasets.</span></span> <span data-ttu-id="61244-513">Vstupní datové sady a datovou sadu výstupů.</span><span class="sxs-lookup"><span data-stu-id="61244-513">An input dataset and an output dataset.</span></span> <span data-ttu-id="61244-514">V tomto návodu, jak používat hello stejný kontejner objektů blob, ale odkazovat toodifferent složky (vstup a výstup).</span><span class="sxs-lookup"><span data-stu-id="61244-514">In this walkthrough, both use hello same blob container but refer toodifferent folders (input and output).</span></span>
 - <span data-ttu-id="61244-515">Kanál.</span><span class="sxs-lookup"><span data-stu-id="61244-515">A pipeline.</span></span> <span data-ttu-id="61244-516">Hello kanál obsahuje aktivitu kopírování, který používá zdroj objektů blob a data toocopy podřízený objekt blob ze objektů blob v Azure umístění tooanother umístění objektu blob Azure.</span><span class="sxs-lookup"><span data-stu-id="61244-516">hello pipeline contains a copy activity that uses a blob source and a blob sink toocopy data from an Azure blob location tooanother Azure blob location.</span></span> 

<span data-ttu-id="61244-517">Hello následující části obsahují další informace o těchto entitách.</span><span class="sxs-lookup"><span data-stu-id="61244-517">hello following sections provide more information about these entities.</span></span> 

#### <a name="linked-services"></a><span data-ttu-id="61244-518">Propojené služby</span><span class="sxs-lookup"><span data-stu-id="61244-518">Linked services</span></span>
<span data-ttu-id="61244-519">Měli byste vidět dvě propojené služby.</span><span class="sxs-lookup"><span data-stu-id="61244-519">You should see two linked services.</span></span> <span data-ttu-id="61244-520">Jednu pro hello zdroje a hello jiného pro cíl hello.</span><span class="sxs-lookup"><span data-stu-id="61244-520">One for hello source and hello other one for hello destination.</span></span> <span data-ttu-id="61244-521">V tomto návodu hello obou definice vzhled stejné s výjimkou hello názvy.</span><span class="sxs-lookup"><span data-stu-id="61244-521">In this walkthrough, both definitions look hello same except for hello names.</span></span> <span data-ttu-id="61244-522">Hello **typ** z hello propojené služby je nastaven příliš**azurestorage**.</span><span class="sxs-lookup"><span data-stu-id="61244-522">hello **type** of hello linked service is set too**AzureStorage**.</span></span> <span data-ttu-id="61244-523">Nejdůležitější vlastnost definice hello propojené služby je hello **connectionString**, který je využíván jiným tooyour tooconnect objekt pro vytváření dat účet služby Azure Storage za běhu.</span><span class="sxs-lookup"><span data-stu-id="61244-523">Most important property of hello linked service definition is hello **connectionString**, which is used by Data Factory tooconnect tooyour Azure Storage account at runtime.</span></span> <span data-ttu-id="61244-524">Ignorujte hello hubName vlastnost v definici hello.</span><span class="sxs-lookup"><span data-stu-id="61244-524">Ignore hello hubName property in hello definition.</span></span> 

##### <a name="source-blob-storage-linked-service"></a><span data-ttu-id="61244-525">Zdrojový objekt blob propojená služba úložiště</span><span class="sxs-lookup"><span data-stu-id="61244-525">Source blob storage linked service</span></span>
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

##### <a name="destination-blob-storage-linked-service"></a><span data-ttu-id="61244-526">Cílový objekt blob propojená služba úložiště</span><span class="sxs-lookup"><span data-stu-id="61244-526">Destination blob storage linked service</span></span>

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

<span data-ttu-id="61244-527">Další informace o propojené služby Azure Storage najdete v tématu [propojené vlastnosti služby](#linked-service-properties) části.</span><span class="sxs-lookup"><span data-stu-id="61244-527">For more information about Azure Storage linked service, see [Linked service properties](#linked-service-properties) section.</span></span> 

#### <a name="datasets"></a><span data-ttu-id="61244-528">Datové sady</span><span class="sxs-lookup"><span data-stu-id="61244-528">Datasets</span></span>
<span data-ttu-id="61244-529">Existují dvě datové sady: vstupní datové sady a datovou sadu výstupů.</span><span class="sxs-lookup"><span data-stu-id="61244-529">There are two datasets: an input dataset and an output dataset.</span></span> <span data-ttu-id="61244-530">Typ Hello sady dat hello je nastaven příliš**AzureBlob** pro obojí.</span><span class="sxs-lookup"><span data-stu-id="61244-530">hello type of hello dataset is set too**AzureBlob** for both.</span></span> 

<span data-ttu-id="61244-531">vstupní datové sady Hello body toohello **vstupní** složky hello **adfblobconnector** kontejner objektů blob.</span><span class="sxs-lookup"><span data-stu-id="61244-531">hello input dataset points toohello **input** folder of hello **adfblobconnector** blob container.</span></span> <span data-ttu-id="61244-532">Hello **externí** vlastnost je nastavena příliš**true** pro tuto datovou sadu jako hello není data vytvořená hello kanálu s aktivitou kopírování hello, která přebírá tuto datovou sadu jako vstup.</span><span class="sxs-lookup"><span data-stu-id="61244-532">hello **external** property is set too**true** for this dataset as hello data is not produced by hello pipeline with hello copy activity that takes this dataset as an input.</span></span> 

<span data-ttu-id="61244-533">Hello výstupní datovou sadu body toohello **výstup** složky hello stejný kontejner objektů blob.</span><span class="sxs-lookup"><span data-stu-id="61244-533">hello output dataset points toohello **output** folder of hello same blob container.</span></span> <span data-ttu-id="61244-534">Hello výstupní datové sady používá také hello rok, měsíc a den hello **SliceStart** proměnné toodynamically systému vyhodnotit hello cesta k souboru výstup hello.</span><span class="sxs-lookup"><span data-stu-id="61244-534">hello output dataset also uses hello year, month, and day of hello **SliceStart** system variable toodynamically evaluate hello path for hello output file.</span></span> <span data-ttu-id="61244-535">Seznam funkcí a systémové proměnné podporovaných službou Data Factory najdete v tématu [funkce pro vytváření dat a systémové proměnné](data-factory-functions-variables.md).</span><span class="sxs-lookup"><span data-stu-id="61244-535">For a list of functions and system variables supported by Data Factory, see [Data Factory functions and system variables](data-factory-functions-variables.md).</span></span> <span data-ttu-id="61244-536">Hello **externí** vlastnost je nastavena příliš**false** (výchozí hodnota) protože tato datová sada je produkovaný hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="61244-536">hello **external** property is set too**false** (default value) because this dataset is produced by hello pipeline.</span></span> 

<span data-ttu-id="61244-537">Další informace o vlastnostech podporovaných zprostředkovatelem datovou sadu objektu Blob Azure najdete v tématu [vlastnosti datové sady](#dataset-properties) části.</span><span class="sxs-lookup"><span data-stu-id="61244-537">For more information about properties supported by Azure Blob dataset, see [Dataset properties](#dataset-properties) section.</span></span>

##### <a name="input-dataset"></a><span data-ttu-id="61244-538">Vstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="61244-538">Input dataset</span></span>

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

##### <a name="output-dataset"></a><span data-ttu-id="61244-539">Výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="61244-539">Output dataset</span></span>

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

#### <a name="pipeline"></a><span data-ttu-id="61244-540">Kanál</span><span class="sxs-lookup"><span data-stu-id="61244-540">Pipeline</span></span>
<span data-ttu-id="61244-541">Hello kanálu má jenom jednu aktivitu.</span><span class="sxs-lookup"><span data-stu-id="61244-541">hello pipeline has just one activity.</span></span> <span data-ttu-id="61244-542">Hello **typ** Dobrý den aktivity je nastavený příliš**kopie**.</span><span class="sxs-lookup"><span data-stu-id="61244-542">hello **type** of hello activity is set too**Copy**.</span></span>  <span data-ttu-id="61244-543">Ve vlastnostech typu hello aktivity hello jsou dvě části, jednu pro zdroje a hello jiného pro sink.</span><span class="sxs-lookup"><span data-stu-id="61244-543">In hello type properties for hello activity, there are two sections, one for source and hello other one for sink.</span></span> <span data-ttu-id="61244-544">Typ zdroje Hello je nastaven příliš**BlobSource** jako aktivita hello je kopírování dat z úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="61244-544">hello source type is set too**BlobSource** as hello activity is copying data from a blob storage.</span></span> <span data-ttu-id="61244-545">Hello typ jímky je nastaven příliš**BlobSink** jako hello aktivita kopírování úložiště objektů blob tooa data.</span><span class="sxs-lookup"><span data-stu-id="61244-545">hello sink type is set too**BlobSink** as hello activity copying data tooa blob storage.</span></span> <span data-ttu-id="61244-546">Aktivita kopírování Hello vezme jako vstupní hello a OutputDataset z4y jako výstup hello InputDataset z4y.</span><span class="sxs-lookup"><span data-stu-id="61244-546">hello copy activity takes InputDataset-z4y as hello input and OutputDataset-z4y as hello output.</span></span> 

<span data-ttu-id="61244-547">Další informace o vlastnostech podporovaných zprostředkovatelem BlobSource a BlobSink najdete v tématu [zkopírovat vlastnosti aktivity](#copy-activity-properties) části.</span><span class="sxs-lookup"><span data-stu-id="61244-547">For more information about properties supported by BlobSource and BlobSink, see [Copy activity properties](#copy-activity-properties) section.</span></span> 

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

## <a name="json-examples-for-copying-data-tooand-from-blob-storage"></a><span data-ttu-id="61244-548">Příklady JSON pro kopírování tooand dat z úložiště objektů Blob</span><span class="sxs-lookup"><span data-stu-id="61244-548">JSON examples for copying data tooand from Blob Storage</span></span>  
<span data-ttu-id="61244-549">Hello následující příklady poskytují definice JSON ukázka používané toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="61244-549">hello following examples provide sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="61244-550">Ukazují jak toocopy tooand dat z Azure Blob Storage a Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="61244-550">They show how toocopy data tooand from Azure Blob Storage and Azure SQL Database.</span></span> <span data-ttu-id="61244-551">Nicméně je možné zkopírovat data **přímo** ze všech zdrojů tooany z hello jímky uvádí [zde](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="61244-551">However, data can be copied **directly** from any of sources tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>

### <a name="json-example-copy-data-from-blob-storage-toosql-database"></a><span data-ttu-id="61244-552">Příklad JSON: Kopírování dat z úložiště objektů Blob tooSQL databáze</span><span class="sxs-lookup"><span data-stu-id="61244-552">JSON Example: Copy data from Blob Storage tooSQL Database</span></span>
<span data-ttu-id="61244-553">Následující ukázka ukazuje Hello:</span><span class="sxs-lookup"><span data-stu-id="61244-553">hello following sample shows:</span></span>

1. <span data-ttu-id="61244-554">Propojené služby typu [azuresqldatabase](data-factory-azure-sql-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="61244-554">A linked service of type [AzureSqlDatabase](data-factory-azure-sql-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="61244-555">Propojené služby typu [azurestorage](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="61244-555">A linked service of type [AzureStorage](#linked-service-properties).</span></span>
3. <span data-ttu-id="61244-556">Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="61244-556">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](#dataset-properties).</span></span>
4. <span data-ttu-id="61244-557">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="61244-557">An output [dataset](data-factory-create-datasets.md) of type [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="61244-558">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [BlobSource](#copy-activity-properties) a [SqlSink](data-factory-azure-sql-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="61244-558">A [pipeline](data-factory-create-pipelines.md) with a Copy activity that uses [BlobSource](#copy-activity-properties) and [SqlSink](data-factory-azure-sql-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="61244-559">Ukázka Hello kopíruje data časové řady z tabulky Azure SQL tooan objektů blob v Azure každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="61244-559">hello sample copies time-series data from an Azure blob tooan Azure SQL table hourly.</span></span> <span data-ttu-id="61244-560">Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.</span><span class="sxs-lookup"><span data-stu-id="61244-560">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="61244-561">**Azure SQL propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="61244-561">**Azure SQL linked service:**</span></span>

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
<span data-ttu-id="61244-562">**Propojená služba Azure Storage:**</span><span class="sxs-lookup"><span data-stu-id="61244-562">**Azure Storage linked service:**</span></span>

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
<span data-ttu-id="61244-563">Podporuje dva typy Azure Storage, propojené služby Azure Data Factory: **azurestorage** a **AzureStorageSas**.</span><span class="sxs-lookup"><span data-stu-id="61244-563">Azure Data Factory supports two types of Azure Storage linked services: **AzureStorage** and **AzureStorageSas**.</span></span> <span data-ttu-id="61244-564">Pro hello první z nich, zadejte hello připojovací řetězec, který obsahuje klíč účtu hello a pro hello později se jeden, zadejte hello Uri sdíleného přístupového podpisu (SAS).</span><span class="sxs-lookup"><span data-stu-id="61244-564">For hello first one, you specify hello connection string that includes hello account key and for hello later one, you specify hello Shared Access Signature (SAS) Uri.</span></span> <span data-ttu-id="61244-565">V tématu [propojené služby](#linked-service-properties) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="61244-565">See [Linked Services](#linked-service-properties) section for details.</span></span>  

<span data-ttu-id="61244-566">**Azure vstupní datovou sadu objektu Blob:**</span><span class="sxs-lookup"><span data-stu-id="61244-566">**Azure Blob input dataset:**</span></span>

<span data-ttu-id="61244-567">Data je převzata z nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="61244-567">Data is picked up from a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="61244-568">název a cesta k souboru složky Hello pro objekt blob hello se vyhodnocují dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="61244-568">hello folder path and file name for hello blob are dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="61244-569">Cesta ke složce Hello používá rok, měsíc a den součástí hello počáteční čas a název souboru používá hello hodinu součástí hello počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="61244-569">hello folder path uses year, month, and day part of hello start time and file name uses hello hour part of hello start time.</span></span> <span data-ttu-id="61244-570">"externí": "PRAVDA" nastavení informuje objekt pro vytváření dat, tato tabulka hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="61244-570">“external”: “true” setting informs Data Factory that hello table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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
<span data-ttu-id="61244-571">**Azure SQL výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="61244-571">**Azure SQL output dataset:**</span></span>

<span data-ttu-id="61244-572">Ukázka Hello zkopíruje data tooa tabulku s názvem "MyTable" v databázi Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="61244-572">hello sample copies data tooa table named “MyTable” in an Azure SQL database.</span></span> <span data-ttu-id="61244-573">Vytvoření hello tabulky v databázi Azure SQL s hello stejný počet sloupců, podle očekávání toocontain soubor Blob CSV hello.</span><span class="sxs-lookup"><span data-stu-id="61244-573">Create hello table in your Azure SQL database with hello same number of columns as you expect hello Blob CSV file toocontain.</span></span> <span data-ttu-id="61244-574">Přidávání řádků tabulky toohello každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="61244-574">New rows are added toohello table every hour.</span></span>

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
<span data-ttu-id="61244-575">**Aktivita kopírování v kanálu s Blob zdroj a jímka SQL:**</span><span class="sxs-lookup"><span data-stu-id="61244-575">**A copy activity in a pipeline with Blob source and SQL sink:**</span></span>

<span data-ttu-id="61244-576">Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="61244-576">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="61244-577">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**BlobSource** a **podřízený** je typ nastaven příliš**SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="61244-577">In hello pipeline JSON definition, hello **source** type is set too**BlobSource** and **sink** type is set too**SqlSink**.</span></span>

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
### <a name="json-example-copy-data-from-azure-sql-tooazure-blob"></a><span data-ttu-id="61244-578">Příklad JSON: Kopírování dat z Azure SQL tooAzure objektů Blob</span><span class="sxs-lookup"><span data-stu-id="61244-578">JSON Example: Copy data from Azure SQL tooAzure Blob</span></span>
<span data-ttu-id="61244-579">Následující ukázka ukazuje Hello:</span><span class="sxs-lookup"><span data-stu-id="61244-579">hello following sample shows:</span></span>

1. <span data-ttu-id="61244-580">Propojené služby typu [azuresqldatabase](data-factory-azure-sql-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="61244-580">A linked service of type [AzureSqlDatabase](data-factory-azure-sql-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="61244-581">Propojené služby typu [azurestorage](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="61244-581">A linked service of type [AzureStorage](#linked-service-properties).</span></span>
3. <span data-ttu-id="61244-582">Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="61244-582">An input [dataset](data-factory-create-datasets.md) of type [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="61244-583">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="61244-583">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](#dataset-properties).</span></span>
5. <span data-ttu-id="61244-584">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [SqlSource](data-factory-azure-sql-connector.md#copy-activity-properties) a [BlobSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="61244-584">A [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [SqlSource](data-factory-azure-sql-connector.md#copy-activity-properties) and [BlobSink](#copy-activity-properties).</span></span>

<span data-ttu-id="61244-585">Ukázka Hello kopíruje data časové řady každou hodinu z tooan tabulky Azure SQL Azure blob.</span><span class="sxs-lookup"><span data-stu-id="61244-585">hello sample copies time-series data from an Azure SQL table tooan Azure blob hourly.</span></span> <span data-ttu-id="61244-586">Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.</span><span class="sxs-lookup"><span data-stu-id="61244-586">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="61244-587">**Azure SQL propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="61244-587">**Azure SQL linked service:**</span></span>

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
<span data-ttu-id="61244-588">**Propojená služba Azure Storage:**</span><span class="sxs-lookup"><span data-stu-id="61244-588">**Azure Storage linked service:**</span></span>

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
<span data-ttu-id="61244-589">Podporuje dva typy Azure Storage, propojené služby Azure Data Factory: **azurestorage** a **AzureStorageSas**.</span><span class="sxs-lookup"><span data-stu-id="61244-589">Azure Data Factory supports two types of Azure Storage linked services: **AzureStorage** and **AzureStorageSas**.</span></span> <span data-ttu-id="61244-590">Pro hello první z nich, zadejte hello připojovací řetězec, který obsahuje klíč účtu hello a pro hello později se jeden, zadejte hello Uri sdíleného přístupového podpisu (SAS).</span><span class="sxs-lookup"><span data-stu-id="61244-590">For hello first one, you specify hello connection string that includes hello account key and for hello later one, you specify hello Shared Access Signature (SAS) Uri.</span></span> <span data-ttu-id="61244-591">V tématu [propojené služby](#linked-service-properties) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="61244-591">See [Linked Services](#linked-service-properties) section for details.</span></span>  

<span data-ttu-id="61244-592">**Azure SQL vstupní datové sady:**</span><span class="sxs-lookup"><span data-stu-id="61244-592">**Azure SQL input dataset:**</span></span>

<span data-ttu-id="61244-593">Ukázka Hello předpokládá jste vytvořili tabulku "MyTable" v Azure SQL a obsahuje sloupec s názvem "timestampcolumn" pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="61244-593">hello sample assumes you have created a table “MyTable” in Azure SQL and it contains a column called “timestampcolumn” for time series data.</span></span>

<span data-ttu-id="61244-594">Nastavení "externí": "PRAVDA" informuje služba Data Factory Tato tabulka hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="61244-594">Setting “external”: ”true” informs Data Factory service that hello table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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

<span data-ttu-id="61244-595">**Azure Blob výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="61244-595">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="61244-596">Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="61244-596">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="61244-597">Cesta ke složce Hello pro objekt blob hello je vyhodnocován dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="61244-597">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="61244-598">Cesta ke složce Hello používá rok, měsíc, den a čas části hello počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="61244-598">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

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

<span data-ttu-id="61244-599">**Aktivita kopírování v kanálu s SQL zdrojový a podřízený objekt Blob:**</span><span class="sxs-lookup"><span data-stu-id="61244-599">**A copy activity in a pipeline with SQL source and Blob sink:**</span></span>

<span data-ttu-id="61244-600">Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="61244-600">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="61244-601">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**SqlSource** a **podřízený** je typ nastaven příliš**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="61244-601">In hello pipeline JSON definition, hello **source** type is set too**SqlSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="61244-602">Dotaz SQL Hello zadaný pro hello **SqlReaderQuery** vlastnost vybere hello data v hello za hodinu toocopy.</span><span class="sxs-lookup"><span data-stu-id="61244-602">hello SQL query specified for hello **SqlReaderQuery** property selects hello data in hello past hour toocopy.</span></span>

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
> <span data-ttu-id="61244-603">toomap sloupce z toocolumns datové sady zdroje z podřízený datové sady, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="61244-603">toomap columns from source dataset toocolumns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="61244-604">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="61244-604">Performance and Tuning</span></span>
<span data-ttu-id="61244-605">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.</span><span class="sxs-lookup"><span data-stu-id="61244-605">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
