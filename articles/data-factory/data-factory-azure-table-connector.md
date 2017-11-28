---
title: aaaMove data do/z Azure Table | Microsoft Docs
description: "Zjistěte, jak toomove data do nebo z úložiště tabulek Azure pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 07b046b1-7884-4e57-a613-337292416319
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jingwang
ms.openlocfilehash: 3dc3da6d88854674a9108b600534bc5d07575f15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooand-from-azure-table-using-azure-data-factory"></a><span data-ttu-id="92950-103">Přesun dat tooand z Azure Table pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="92950-103">Move data tooand from Azure Table using Azure Data Factory</span></span>
<span data-ttu-id="92950-104">Tento článek vysvětluje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove dat z úložiště tabulek Azure.</span><span class="sxs-lookup"><span data-stu-id="92950-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data to/from Azure Table Storage.</span></span> <span data-ttu-id="92950-105">Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="92950-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span> 

<span data-ttu-id="92950-106">Data můžete zkopírovat z jakéhokoli podporované zdroje dat ukládání tooAzure Table Storage nebo z Azure Table Storage tooany podporované jímku dat úložiště.</span><span class="sxs-lookup"><span data-stu-id="92950-106">You can copy data from any supported source data store tooAzure Table Storage or from Azure Table Storage tooany supported sink data store.</span></span> <span data-ttu-id="92950-107">Seznam úložišť dat jako zdroje nebo jímky podporované aktivitou kopírování hello najdete v tématu hello [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky.</span><span class="sxs-lookup"><span data-stu-id="92950-107">For a list of data stores supported as sources or sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> 

## <a name="getting-started"></a><span data-ttu-id="92950-108">Začínáme</span><span class="sxs-lookup"><span data-stu-id="92950-108">Getting started</span></span>
<span data-ttu-id="92950-109">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z Azure Table Storage pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="92950-109">You can create a pipeline with a copy activity that moves data to/from an Azure Table Storage by using different tools/APIs.</span></span>

<span data-ttu-id="92950-110">Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="92950-110">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="92950-111">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.</span><span class="sxs-lookup"><span data-stu-id="92950-111">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="92950-112">Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**.</span><span class="sxs-lookup"><span data-stu-id="92950-112">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="92950-113">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="92950-113">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="92950-114">Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:</span><span class="sxs-lookup"><span data-stu-id="92950-114">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span> 

1. <span data-ttu-id="92950-115">Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="92950-115">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="92950-116">Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="92950-116">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="92950-117">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="92950-117">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="92950-118">Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello).</span><span class="sxs-lookup"><span data-stu-id="92950-118">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="92950-119">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.</span><span class="sxs-lookup"><span data-stu-id="92950-119">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="92950-120">Ukázky s definicemi JSON entit služby Data Factory, které jsou používané toocopy data do nebo ze Azure Table Storage, najdete v části [JSON příklady](#json-examples) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="92950-120">For samples with JSON definitions for Data Factory entities that are used toocopy data to/from an Azure Table Storage, see [JSON examples](#json-examples) section of this article.</span></span> 

<span data-ttu-id="92950-121">Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooAzure Table Storage:</span><span class="sxs-lookup"><span data-stu-id="92950-121">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooAzure Table Storage:</span></span> 

## <a name="linked-service-properties"></a><span data-ttu-id="92950-122">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="92950-122">Linked service properties</span></span>
<span data-ttu-id="92950-123">Existují dva typy propojené služby můžete použít toolink služby Azure blob storage tooan Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="92950-123">There are two types of linked services you can use toolink an Azure blob storage tooan Azure data factory.</span></span> <span data-ttu-id="92950-124">Jsou: **azurestorage** propojená služba a **AzureStorageSas** propojené služby.</span><span class="sxs-lookup"><span data-stu-id="92950-124">They are: **AzureStorage** linked service and **AzureStorageSas** linked service.</span></span> <span data-ttu-id="92950-125">Hello propojené služby Azure Storage poskytuje objekt pro vytváření dat hello s globálním přístupem toohello Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="92950-125">hello Azure Storage linked service provides hello data factory with global access toohello Azure Storage.</span></span> <span data-ttu-id="92950-126">Zatímco hello Azure úložiště SAS (sdíleného přístupového podpisu) propojená služba poskytuje objekt pro vytváření dat hello s přístup omezený nebo časově vázaných toohello Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="92950-126">Whereas, hello Azure Storage SAS (Shared Access Signature) linked service provides hello data factory with restricted/time-bound access toohello Azure Storage.</span></span> <span data-ttu-id="92950-127">Nejsou žádné další rozdíly mezi tyto dvě propojené služby.</span><span class="sxs-lookup"><span data-stu-id="92950-127">There are no other differences between these two linked services.</span></span> <span data-ttu-id="92950-128">Zvolte hello propojené služby, který vyhovuje vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="92950-128">Choose hello linked service that suits your needs.</span></span> <span data-ttu-id="92950-129">Hello následující oddíly poskytují další podrobnosti o tyto dvě propojené služby.</span><span class="sxs-lookup"><span data-stu-id="92950-129">hello following sections provide more details on these two linked services.</span></span>

[!INCLUDE [data-factory-azure-storage-linked-services](../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="dataset-properties"></a><span data-ttu-id="92950-130">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="92950-130">Dataset properties</span></span>
<span data-ttu-id="92950-131">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="92950-131">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="92950-132">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="92950-132">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="92950-133">část rámci typeProperties Hello se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello.</span><span class="sxs-lookup"><span data-stu-id="92950-133">hello typeProperties section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="92950-134">Hello **rámci typeProperties** části pro datovou sadu hello typu **AzureTable** má následující vlastnosti hello.</span><span class="sxs-lookup"><span data-stu-id="92950-134">hello **typeProperties** section for hello dataset of type **AzureTable** has hello following properties.</span></span>

| <span data-ttu-id="92950-135">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="92950-135">Property</span></span> | <span data-ttu-id="92950-136">Popis</span><span class="sxs-lookup"><span data-stu-id="92950-136">Description</span></span> | <span data-ttu-id="92950-137">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="92950-137">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="92950-138">tableName</span><span class="sxs-lookup"><span data-stu-id="92950-138">tableName</span></span> |<span data-ttu-id="92950-139">Název tabulky hello hello instance Azure tabulku databáze, kterou propojená služba odkazuje.</span><span class="sxs-lookup"><span data-stu-id="92950-139">Name of hello table in hello Azure Table Database instance that linked service refers to.</span></span> |<span data-ttu-id="92950-140">Ano.</span><span class="sxs-lookup"><span data-stu-id="92950-140">Yes.</span></span> <span data-ttu-id="92950-141">Když název tabulky je zadán bez azureTableSourceQuery, jsou všechny záznamy z tabulky hello zkopírovaný toohello cílový.</span><span class="sxs-lookup"><span data-stu-id="92950-141">When a tableName is specified without an azureTableSourceQuery, all records from hello table are copied toohello destination.</span></span> <span data-ttu-id="92950-142">Pokud je zadána také azureTableSourceQuery, záznamy z hello tabulky, který splňuje hello dotazu jsou zkopírované toohello cílový.</span><span class="sxs-lookup"><span data-stu-id="92950-142">If an azureTableSourceQuery is also specified, records from hello table that satisfies hello query are copied toohello destination.</span></span> |

### <a name="schema-by-data-factory"></a><span data-ttu-id="92950-143">Schéma službou Data Factory</span><span class="sxs-lookup"><span data-stu-id="92950-143">Schema by Data Factory</span></span>
<span data-ttu-id="92950-144">Pro data bez schémat úložiště, jako je například Azure Table odvodí hello služba Data Factory hello schéma v jednom z následujících způsobů hello:</span><span class="sxs-lookup"><span data-stu-id="92950-144">For schema-free data stores such as Azure Table, hello Data Factory service infers hello schema in one of hello following ways:</span></span>

1. <span data-ttu-id="92950-145">Pokud zadáte hello strukturu dat pomocí hello **struktura** vlastnost v definici datové sady hello, hello služba Data Factory ctí tato struktura jako hello schématu.</span><span class="sxs-lookup"><span data-stu-id="92950-145">If you specify hello structure of data by using hello **structure** property in hello dataset definition, hello Data Factory service honors this structure as hello schema.</span></span> <span data-ttu-id="92950-146">V takovém případě Pokud řádek neobsahuje hodnotu pro sloupec, je pro něj zadat hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="92950-146">In this case, if a row does not contain a value for a column, a null value is provided for it.</span></span>
2. <span data-ttu-id="92950-147">Pokud nezadáte hello strukturu dat pomocí hello **struktura** vlastnost v definici datové sady hello, Data Factory odvodí hello schématu pomocí hello první řádek v datech hello.</span><span class="sxs-lookup"><span data-stu-id="92950-147">If you don't specify hello structure of data by using hello **structure** property in hello dataset definition, Data Factory infers hello schema by using hello first row in hello data.</span></span> <span data-ttu-id="92950-148">V takovém případě pokud první řádek hello neobsahuje úplnou schématu hello, některé sloupce naplánované v hello výsledek operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="92950-148">In this case, if hello first row does not contain hello full schema, some columns are missed in hello result of copy operation.</span></span>

<span data-ttu-id="92950-149">Proto pro zdroje dat bez schémat, hello osvědčeným postupem je toospecify hello strukturu dat pomocí hello **struktura** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="92950-149">Therefore, for schema-free data sources, hello best practice is toospecify hello structure of data using hello **structure** property.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="92950-150">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="92950-150">Copy activity properties</span></span>
<span data-ttu-id="92950-151">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="92950-151">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="92950-152">Vlastnosti, například název, popis, vstupní a výstupní datové sady a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="92950-152">Properties such as name, description, input and output datasets, and policies are available for all types of activities.</span></span>

<span data-ttu-id="92950-153">Vlastnosti dostupné v rámci typeProperties části hello hello aktivit na hello druhé straně lišit každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="92950-153">Properties available in hello typeProperties section of hello activity on hello other hand vary with each activity type.</span></span> <span data-ttu-id="92950-154">Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="92950-154">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="92950-155">**AzureTableSource** podporuje hello následující vlastnosti v rámci typeProperties části:</span><span class="sxs-lookup"><span data-stu-id="92950-155">**AzureTableSource** supports hello following properties in typeProperties section:</span></span>

| <span data-ttu-id="92950-156">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="92950-156">Property</span></span> | <span data-ttu-id="92950-157">Popis</span><span class="sxs-lookup"><span data-stu-id="92950-157">Description</span></span> | <span data-ttu-id="92950-158">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="92950-158">Allowed values</span></span> | <span data-ttu-id="92950-159">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="92950-159">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="92950-160">azureTableSourceQuery</span><span class="sxs-lookup"><span data-stu-id="92950-160">azureTableSourceQuery</span></span> |<span data-ttu-id="92950-161">Použijte data tooread hello vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="92950-161">Use hello custom query tooread data.</span></span> |<span data-ttu-id="92950-162">Řetězec dotazu tabulky Azure.</span><span class="sxs-lookup"><span data-stu-id="92950-162">Azure table query string.</span></span> <span data-ttu-id="92950-163">Příklady v další části hello.</span><span class="sxs-lookup"><span data-stu-id="92950-163">See examples in hello next section.</span></span> |<span data-ttu-id="92950-164">Ne.</span><span class="sxs-lookup"><span data-stu-id="92950-164">No.</span></span> <span data-ttu-id="92950-165">Když název tabulky je zadán bez azureTableSourceQuery, jsou všechny záznamy z tabulky hello zkopírovaný toohello cílový.</span><span class="sxs-lookup"><span data-stu-id="92950-165">When a tableName is specified without an azureTableSourceQuery, all records from hello table are copied toohello destination.</span></span> <span data-ttu-id="92950-166">Pokud je zadána také azureTableSourceQuery, záznamy z hello tabulky, který splňuje hello dotazu jsou zkopírované toohello cílový.</span><span class="sxs-lookup"><span data-stu-id="92950-166">If an azureTableSourceQuery is also specified, records from hello table that satisfies hello query are copied toohello destination.</span></span> |
| <span data-ttu-id="92950-167">azureTableSourceIgnoreTableNotFound</span><span class="sxs-lookup"><span data-stu-id="92950-167">azureTableSourceIgnoreTableNotFound</span></span> |<span data-ttu-id="92950-168">Označuje, zda swallow hello výjimky tabulky neexistuje.</span><span class="sxs-lookup"><span data-stu-id="92950-168">Indicate whether swallow hello exception of table not exist.</span></span> |<span data-ttu-id="92950-169">HODNOTA TRUE</span><span class="sxs-lookup"><span data-stu-id="92950-169">TRUE</span></span><br/><span data-ttu-id="92950-170">FALSE</span><span class="sxs-lookup"><span data-stu-id="92950-170">FALSE</span></span> |<span data-ttu-id="92950-171">Ne</span><span class="sxs-lookup"><span data-stu-id="92950-171">No</span></span> |

### <a name="azuretablesourcequery-examples"></a><span data-ttu-id="92950-172">Příklady azureTableSourceQuery</span><span class="sxs-lookup"><span data-stu-id="92950-172">azureTableSourceQuery examples</span></span>
<span data-ttu-id="92950-173">Pokud sloupec tabulky Azure je typu řetězec:</span><span class="sxs-lookup"><span data-stu-id="92950-173">If Azure Table column is of string type:</span></span>

```JSON
azureTableSourceQuery": "$$Text.Format('PartitionKey ge \\'{0:yyyyMMddHH00_0000}\\' and PartitionKey le \\'{0:yyyyMMddHH00_9999}\\'', SliceStart)"
```

<span data-ttu-id="92950-174">Pokud sloupec tabulky Azure je typu datum a čas:</span><span class="sxs-lookup"><span data-stu-id="92950-174">If Azure Table column is of datetime type:</span></span>

```JSON
"azureTableSourceQuery": "$$Text.Format('DeploymentEndTime gt datetime\\'{0:yyyy-MM-ddTHH:mm:ssZ}\\' and DeploymentEndTime le datetime\\'{1:yyyy-MM-ddTHH:mm:ssZ}\\'', SliceStart, SliceEnd)"
```

<span data-ttu-id="92950-175">**AzureTableSink** podporuje hello následující vlastnosti v rámci typeProperties části:</span><span class="sxs-lookup"><span data-stu-id="92950-175">**AzureTableSink** supports hello following properties in typeProperties section:</span></span>

| <span data-ttu-id="92950-176">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="92950-176">Property</span></span> | <span data-ttu-id="92950-177">Popis</span><span class="sxs-lookup"><span data-stu-id="92950-177">Description</span></span> | <span data-ttu-id="92950-178">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="92950-178">Allowed values</span></span> | <span data-ttu-id="92950-179">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="92950-179">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="92950-180">azureTableDefaultPartitionKeyValue</span><span class="sxs-lookup"><span data-stu-id="92950-180">azureTableDefaultPartitionKeyValue</span></span> |<span data-ttu-id="92950-181">Výchozí hodnotu klíče oddílu, mohou být využívána hello jímky.</span><span class="sxs-lookup"><span data-stu-id="92950-181">Default partition key value that can be used by hello sink.</span></span> |<span data-ttu-id="92950-182">Hodnotu řetězce.</span><span class="sxs-lookup"><span data-stu-id="92950-182">A string value.</span></span> |<span data-ttu-id="92950-183">Ne</span><span class="sxs-lookup"><span data-stu-id="92950-183">No</span></span> |
| <span data-ttu-id="92950-184">azureTablePartitionKeyName</span><span class="sxs-lookup"><span data-stu-id="92950-184">azureTablePartitionKeyName</span></span> |<span data-ttu-id="92950-185">Zadejte název hello sloupce, jejichž hodnoty se používají jako klíče oddílů.</span><span class="sxs-lookup"><span data-stu-id="92950-185">Specify name of hello column whose values are used as partition keys.</span></span> <span data-ttu-id="92950-186">Pokud není zadaný, použije se jako klíč oddílu hello AzureTableDefaultPartitionKeyValue.</span><span class="sxs-lookup"><span data-stu-id="92950-186">If not specified, AzureTableDefaultPartitionKeyValue is used as hello partition key.</span></span> |<span data-ttu-id="92950-187">Název sloupce.</span><span class="sxs-lookup"><span data-stu-id="92950-187">A column name.</span></span> |<span data-ttu-id="92950-188">Ne</span><span class="sxs-lookup"><span data-stu-id="92950-188">No</span></span> |
| <span data-ttu-id="92950-189">azureTableRowKeyName</span><span class="sxs-lookup"><span data-stu-id="92950-189">azureTableRowKeyName</span></span> |<span data-ttu-id="92950-190">Zadejte název hello sloupce, jejichž hodnoty sloupce jsou použity jako klíč řádku.</span><span class="sxs-lookup"><span data-stu-id="92950-190">Specify name of hello column whose column values are used as row key.</span></span> <span data-ttu-id="92950-191">Pokud není zadaný, použijte identifikátor GUID pro každý řádek.</span><span class="sxs-lookup"><span data-stu-id="92950-191">If not specified, use a GUID for each row.</span></span> |<span data-ttu-id="92950-192">Název sloupce.</span><span class="sxs-lookup"><span data-stu-id="92950-192">A column name.</span></span> |<span data-ttu-id="92950-193">Ne</span><span class="sxs-lookup"><span data-stu-id="92950-193">No</span></span> |
| <span data-ttu-id="92950-194">azureTableInsertType</span><span class="sxs-lookup"><span data-stu-id="92950-194">azureTableInsertType</span></span> |<span data-ttu-id="92950-195">Hello režimu tooinsert data do tabulky Azure.</span><span class="sxs-lookup"><span data-stu-id="92950-195">hello mode tooinsert data into Azure table.</span></span><br/><br/><span data-ttu-id="92950-196">Tato vlastnost určuje, jestli mají existující řádky v tabulce výstup hello s odpovídajícím klíče oddílu a řádku jejich hodnoty nahradit nebo sloučit.</span><span class="sxs-lookup"><span data-stu-id="92950-196">This property controls whether existing rows in hello output table with matching partition and row keys have their values replaced or merged.</span></span> <br/><br/><span data-ttu-id="92950-197">toolearn o tom, jak tato nastavení (sloučení a nahraďte) fungují, najdete v části [vložení nebo sloučení Entity](https://msdn.microsoft.com/library/azure/hh452241.aspx) a [vložení nebo nahrazení Entity](https://msdn.microsoft.com/library/azure/hh452242.aspx) témata.</span><span class="sxs-lookup"><span data-stu-id="92950-197">toolearn about how these settings (merge and replace) work, see [Insert or Merge Entity](https://msdn.microsoft.com/library/azure/hh452241.aspx) and [Insert or Replace Entity](https://msdn.microsoft.com/library/azure/hh452242.aspx) topics.</span></span> <br/><br> <span data-ttu-id="92950-198">Toto nastavení se vztahuje na úrovni řádku hello, není hello na úrovni tabulky, a ani možnost odstraní řádky v tabulce výstup hello, které nejsou k dispozici ve vstupu hello.</span><span class="sxs-lookup"><span data-stu-id="92950-198">This setting applies at hello row level, not hello table level, and neither option deletes rows in hello output table that do not exist in hello input.</span></span> |<span data-ttu-id="92950-199">Merge (výchozí)</span><span class="sxs-lookup"><span data-stu-id="92950-199">merge (default)</span></span><br/><span data-ttu-id="92950-200">Nahradit</span><span class="sxs-lookup"><span data-stu-id="92950-200">replace</span></span> |<span data-ttu-id="92950-201">Ne</span><span class="sxs-lookup"><span data-stu-id="92950-201">No</span></span> |
| <span data-ttu-id="92950-202">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="92950-202">writeBatchSize</span></span> |<span data-ttu-id="92950-203">Když je dosaženo hello writeBatchSize nebo writeBatchTimeout vkládá data do hello tabulky Azure.</span><span class="sxs-lookup"><span data-stu-id="92950-203">Inserts data into hello Azure table when hello writeBatchSize or writeBatchTimeout is hit.</span></span> |<span data-ttu-id="92950-204">Celé číslo (počet řádků)</span><span class="sxs-lookup"><span data-stu-id="92950-204">Integer (number of rows)</span></span> |<span data-ttu-id="92950-205">Ne (výchozí: 10000)</span><span class="sxs-lookup"><span data-stu-id="92950-205">No (default: 10000)</span></span> |
| <span data-ttu-id="92950-206">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="92950-206">writeBatchTimeout</span></span> |<span data-ttu-id="92950-207">Když je dosaženo hello writeBatchSize nebo writeBatchTimeout vkládá data do hello tabulky Azure</span><span class="sxs-lookup"><span data-stu-id="92950-207">Inserts data into hello Azure table when hello writeBatchSize or writeBatchTimeout is hit</span></span> |<span data-ttu-id="92950-208">Časový interval</span><span class="sxs-lookup"><span data-stu-id="92950-208">timespan</span></span><br/><br/><span data-ttu-id="92950-209">Příklad: "00:20:00" (20 minut)</span><span class="sxs-lookup"><span data-stu-id="92950-209">Example: “00:20:00” (20 minutes)</span></span> |<span data-ttu-id="92950-210">Ne (výchozí hodnota časového limitu pro klienta toostorage výchozí hodnotu 90 sekundu)</span><span class="sxs-lookup"><span data-stu-id="92950-210">No (Default toostorage client default timeout value 90 sec)</span></span> |

### <a name="azuretablepartitionkeyname"></a><span data-ttu-id="92950-211">azureTablePartitionKeyName</span><span class="sxs-lookup"><span data-stu-id="92950-211">azureTablePartitionKeyName</span></span>
<span data-ttu-id="92950-212">Mapovat zdrojový sloupec tooa cílový sloupec pomocí hello překladač JSON vlastnost jako hello azureTablePartitionKeyName, abyste mohli používat hello cílový sloupec.</span><span class="sxs-lookup"><span data-stu-id="92950-212">Map a source column tooa destination column using hello translator JSON property before you can use hello destination column as hello azureTablePartitionKeyName.</span></span>

<span data-ttu-id="92950-213">V následujícím příkladu hello, zdrojový sloupec DivisionID je namapované toohello cílový sloupec: DivisionID.</span><span class="sxs-lookup"><span data-stu-id="92950-213">In hello following example, source column DivisionID is mapped toohello destination column: DivisionID.</span></span>  

```JSON
"translator": {
    "type": "TabularTranslator",
    "columnMappings": "DivisionID: DivisionID, FirstName: FirstName, LastName: LastName"
}
```
<span data-ttu-id="92950-214">Hello DivisionID je zadán jako klíč oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="92950-214">hello DivisionID is specified as hello partition key.</span></span>

```JSON
"sink": {
    "type": "AzureTableSink",
    "azureTablePartitionKeyName": "DivisionID",
    "writeBatchSize": 100,
    "writeBatchTimeout": "01:00:00"
}
```
## <a name="json-examples"></a><span data-ttu-id="92950-215">Příklady JSON</span><span class="sxs-lookup"><span data-stu-id="92950-215">JSON examples</span></span>
<span data-ttu-id="92950-216">Hello následující příklady poskytují definice JSON ukázka používané toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="92950-216">hello following examples provide sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="92950-217">Ukazují jak toocopy tooand dat z úložiště tabulek Azure a Azure Blob databáze.</span><span class="sxs-lookup"><span data-stu-id="92950-217">They show how toocopy data tooand from Azure Table Storage and Azure Blob Database.</span></span> <span data-ttu-id="92950-218">Nicméně je možné zkopírovat data **přímo** z jakéhokoli z tooany zdroje hello z jímky hello podporována.</span><span class="sxs-lookup"><span data-stu-id="92950-218">However, data can be copied **directly** from any of hello sources tooany of hello supported sinks.</span></span> <span data-ttu-id="92950-219">Další informace najdete v tématu hello části "podporované úložiště dat a formáty" v [přesun dat pomocí aktivity kopírování](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="92950-219">For more information, see hello section "Supported data stores and formats" in [Move data by using Copy Activity](data-factory-data-movement-activities.md).</span></span>

## <a name="example-copy-data-from-azure-table-tooazure-blob"></a><span data-ttu-id="92950-220">Příklad: Kopírování dat z Azure Table tooAzure objektů Blob</span><span class="sxs-lookup"><span data-stu-id="92950-220">Example: Copy data from Azure Table tooAzure Blob</span></span>
<span data-ttu-id="92950-221">Následující ukázka ukazuje Hello:</span><span class="sxs-lookup"><span data-stu-id="92950-221">hello following sample shows:</span></span>

1. <span data-ttu-id="92950-222">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties) (používá se pro objekt blob & tabulky).</span><span class="sxs-lookup"><span data-stu-id="92950-222">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties) (used for both table & blob).</span></span>
2. <span data-ttu-id="92950-223">Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="92950-223">An input [dataset](data-factory-create-datasets.md) of type [AzureTable](#dataset-properties).</span></span>
3. <span data-ttu-id="92950-224">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="92950-224">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="92950-225">Hello [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [AzureTableSource](#activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="92950-225">hello [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [AzureTableSource](#activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="92950-226">Ukázka Hello zkopíruje data patřící toohello výchozí oddíl v objektu blob Azure Table tooa každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="92950-226">hello sample copies data belonging toohello default partition in an Azure Table tooa blob every hour.</span></span> <span data-ttu-id="92950-227">Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.</span><span class="sxs-lookup"><span data-stu-id="92950-227">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="92950-228">**Propojená služba úložiště Azure:**</span><span class="sxs-lookup"><span data-stu-id="92950-228">**Azure storage linked service:**</span></span>

```JSON
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
<span data-ttu-id="92950-229">Podporuje dva typy Azure Storage, propojené služby Azure Data Factory: **azurestorage** a **AzureStorageSas**.</span><span class="sxs-lookup"><span data-stu-id="92950-229">Azure Data Factory supports two types of Azure Storage linked services: **AzureStorage** and **AzureStorageSas**.</span></span> <span data-ttu-id="92950-230">Pro hello první z nich, zadejte hello připojovací řetězec, který obsahuje klíč účtu hello a pro hello později se jeden, zadejte hello Uri sdíleného přístupového podpisu (SAS).</span><span class="sxs-lookup"><span data-stu-id="92950-230">For hello first one, you specify hello connection string that includes hello account key and for hello later one, you specify hello Shared Access Signature (SAS) Uri.</span></span> <span data-ttu-id="92950-231">V tématu [propojené služby](#linked-service-properties) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="92950-231">See [Linked Services](#linked-service-properties) section for details.</span></span>  

<span data-ttu-id="92950-232">**Azure Table vstupní datové sady:**</span><span class="sxs-lookup"><span data-stu-id="92950-232">**Azure Table input dataset:**</span></span>

<span data-ttu-id="92950-233">Ukázka Hello předpokládá, že jste vytvořili tabulku "MyTable" v Azure Table.</span><span class="sxs-lookup"><span data-stu-id="92950-233">hello sample assumes you have created a table “MyTable” in Azure Table.</span></span>

<span data-ttu-id="92950-234">Nastavení "externí": "PRAVDA" informuje služba Data Factory hello tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="92950-234">Setting “external”: ”true” informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```JSON
{
  "name": "AzureTableInput",
  "properties": {
    "type": "AzureTable",
    "linkedServiceName": "StorageLinkedService",
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

<span data-ttu-id="92950-235">**Azure Blob výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="92950-235">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="92950-236">Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="92950-236">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="92950-237">Cesta ke složce Hello pro objekt blob hello je vyhodnocován dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="92950-237">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="92950-238">Cesta ke složce Hello používá rok, měsíc, den a čas části hello počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="92950-238">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

<span data-ttu-id="92950-239">**Aktivita kopírování v kanálu s AzureTableSource a BlobSink:**</span><span class="sxs-lookup"><span data-stu-id="92950-239">**Copy activity in a pipeline with AzureTableSource and BlobSink:**</span></span>

<span data-ttu-id="92950-240">Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="92950-240">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="92950-241">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**AzureTableSource** a **podřízený** je typ nastaven příliš**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="92950-241">In hello pipeline JSON definition, hello **source** type is set too**AzureTableSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="92950-242">Zadaný dotaz SQL Hello **AzureTableSourceQuery** vlastnost vybere hello dat z oddílu výchozí hello toocopy každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="92950-242">hello SQL query specified with **AzureTableSourceQuery** property selects hello data from hello default partition every hour toocopy.</span></span>

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
            {
                "name": "AzureTabletoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                      {
                        "name": "AzureTableInput"
                    }
                ],
                "outputs": [
                      {
                            "name": "AzureBlobOutput"
                      }
                ],
                "typeProperties": {
                      "source": {
                        "type": "AzureTableSource",
                        "AzureTableSourceQuery": "PartitionKey eq 'DefaultPartitionKey'"
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

## <a name="example-copy-data-from-azure-blob-tooazure-table"></a><span data-ttu-id="92950-243">Příklad: Kopírování dat z Azure Blob tooAzure tabulky</span><span class="sxs-lookup"><span data-stu-id="92950-243">Example: Copy data from Azure Blob tooAzure Table</span></span>
<span data-ttu-id="92950-244">Následující ukázka ukazuje Hello:</span><span class="sxs-lookup"><span data-stu-id="92950-244">hello following sample shows:</span></span>

1. <span data-ttu-id="92950-245">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties) (používá se pro objekt blob & tabulky)</span><span class="sxs-lookup"><span data-stu-id="92950-245">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties) (used for both table & blob)</span></span>
2. <span data-ttu-id="92950-246">Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="92950-246">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
3. <span data-ttu-id="92950-247">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="92950-247">An output [dataset](data-factory-create-datasets.md) of type [AzureTable](#dataset-properties).</span></span>
4. <span data-ttu-id="92950-248">Hello [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) a [AzureTableSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="92950-248">hello [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) and [AzureTableSink](#copy-activity-properties).</span></span>

<span data-ttu-id="92950-249">Ukázka Hello zkopíruje data časové řady ze objektů blob v Azure tooan tabulky Azure každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="92950-249">hello sample copies time-series data from an Azure blob tooan Azure table hourly.</span></span> <span data-ttu-id="92950-250">Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.</span><span class="sxs-lookup"><span data-stu-id="92950-250">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="92950-251">**Propojená služba Azure storage (pro Azure Table & objektů Blob):**</span><span class="sxs-lookup"><span data-stu-id="92950-251">**Azure storage (for both Azure Table & Blob) linked service:**</span></span>

```JSON
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

<span data-ttu-id="92950-252">Podporuje dva typy Azure Storage, propojené služby Azure Data Factory: **azurestorage** a **AzureStorageSas**.</span><span class="sxs-lookup"><span data-stu-id="92950-252">Azure Data Factory supports two types of Azure Storage linked services: **AzureStorage** and **AzureStorageSas**.</span></span> <span data-ttu-id="92950-253">Pro hello první z nich, zadejte hello připojovací řetězec, který obsahuje klíč účtu hello a pro hello později se jeden, zadejte hello Uri sdíleného přístupového podpisu (SAS).</span><span class="sxs-lookup"><span data-stu-id="92950-253">For hello first one, you specify hello connection string that includes hello account key and for hello later one, you specify hello Shared Access Signature (SAS) Uri.</span></span> <span data-ttu-id="92950-254">V tématu [propojené služby](#linked-service-properties) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="92950-254">See [Linked Services](#linked-service-properties) section for details.</span></span>

<span data-ttu-id="92950-255">**Azure vstupní datovou sadu objektu Blob:**</span><span class="sxs-lookup"><span data-stu-id="92950-255">**Azure Blob input dataset:**</span></span>

<span data-ttu-id="92950-256">Data je převzata z nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="92950-256">Data is picked up from a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="92950-257">název a cesta k souboru složky Hello pro objekt blob hello se vyhodnocují dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="92950-257">hello folder path and file name for hello blob are dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="92950-258">Cesta ke složce Hello používá rok, měsíc a den součástí hello počáteční čas a název souboru používá hello hodinu součástí hello počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="92950-258">hello folder path uses year, month, and day part of hello start time and file name uses hello hour part of hello start time.</span></span> <span data-ttu-id="92950-259">"externí": "PRAVDA" nastavení informuje služba Data Factory hello tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="92950-259">“external”: “true” setting informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```JSON
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
      "fileName": "{Hour}.csv",
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

<span data-ttu-id="92950-260">**Tabulky Azure výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="92950-260">**Azure Table output dataset:**</span></span>

<span data-ttu-id="92950-261">Ukázka Hello zkopíruje data tooa tabulku s názvem "MyTable" v tabulce Azure.</span><span class="sxs-lookup"><span data-stu-id="92950-261">hello sample copies data tooa table named “MyTable” in Azure Table.</span></span> <span data-ttu-id="92950-262">Vytvoření Azure tabulky s hello stejný počet sloupců, podle očekávání toocontain soubor Blob CSV hello.</span><span class="sxs-lookup"><span data-stu-id="92950-262">Create an Azure table with hello same number of columns as you expect hello Blob CSV file toocontain.</span></span> <span data-ttu-id="92950-263">Přidávání řádků tabulky toohello každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="92950-263">New rows are added toohello table every hour.</span></span>

```JSON
{
  "name": "AzureTableOutput",
  "properties": {
    "type": "AzureTable",
    "linkedServiceName": "StorageLinkedService",
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

<span data-ttu-id="92950-264">**Aktivita kopírování v kanálu s BlobSource a AzureTableSink:**</span><span class="sxs-lookup"><span data-stu-id="92950-264">**Copy activity in a pipeline with BlobSource and AzureTableSink:**</span></span>

<span data-ttu-id="92950-265">Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="92950-265">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="92950-266">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**BlobSource** a **podřízený** je typ nastaven příliš**AzureTableSink**.</span><span class="sxs-lookup"><span data-stu-id="92950-266">In hello pipeline JSON definition, hello **source** type is set too**BlobSource** and **sink** type is set too**AzureTableSink**.</span></span>

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoTable",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureTableOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "AzureTableSink",
            "writeBatchSize": 100,
            "writeBatchTimeout": "01:00:00"
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
## <a name="type-mapping-for-azure-table"></a><span data-ttu-id="92950-267">Typ mapování pro tabulku Azure</span><span class="sxs-lookup"><span data-stu-id="92950-267">Type Mapping for Azure Table</span></span>
<span data-ttu-id="92950-268">Jak je uvedeno v hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů toosink typy zdroje s hello následující dvoustupňový přístup.</span><span class="sxs-lookup"><span data-stu-id="92950-268">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types toosink types with hello following two-step approach.</span></span>

1. <span data-ttu-id="92950-269">Převod z typu too.NET typy nativní zdroje</span><span class="sxs-lookup"><span data-stu-id="92950-269">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="92950-270">Převést typ jímky toonative typ rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="92950-270">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="92950-271">Při přesouvání dat příliš & z tabulky Azure, hello následující [mapování definovaná službou Azure Table](https://msdn.microsoft.com/library/azure/dd179338.aspx) se používají z typu too.NET typy OData tabulky Azure a naopak.</span><span class="sxs-lookup"><span data-stu-id="92950-271">When moving data too& from Azure Table, hello following [mappings defined by Azure Table service](https://msdn.microsoft.com/library/azure/dd179338.aspx) are used from Azure Table OData types too.NET type and vice versa.</span></span>

| <span data-ttu-id="92950-272">Typ dat OData</span><span class="sxs-lookup"><span data-stu-id="92950-272">OData Data Type</span></span> | <span data-ttu-id="92950-273">Typ formátu .NET</span><span class="sxs-lookup"><span data-stu-id="92950-273">.NET Type</span></span> | <span data-ttu-id="92950-274">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="92950-274">Details</span></span> |
| --- | --- | --- |
| <span data-ttu-id="92950-275">Edm.Binary</span><span class="sxs-lookup"><span data-stu-id="92950-275">Edm.Binary</span></span> |<span data-ttu-id="92950-276">Byte]</span><span class="sxs-lookup"><span data-stu-id="92950-276">byte[]</span></span> |<span data-ttu-id="92950-277">Pole bajtů až too64 KB.</span><span class="sxs-lookup"><span data-stu-id="92950-277">An array of bytes up too64 KB.</span></span> |
| <span data-ttu-id="92950-278">Edm.Boolean</span><span class="sxs-lookup"><span data-stu-id="92950-278">Edm.Boolean</span></span> |<span data-ttu-id="92950-279">BOOL</span><span class="sxs-lookup"><span data-stu-id="92950-279">bool</span></span> |<span data-ttu-id="92950-280">Logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="92950-280">A Boolean value.</span></span> |
| <span data-ttu-id="92950-281">Edm.DateTime</span><span class="sxs-lookup"><span data-stu-id="92950-281">Edm.DateTime</span></span> |<span data-ttu-id="92950-282">Data a času</span><span class="sxs-lookup"><span data-stu-id="92950-282">DateTime</span></span> |<span data-ttu-id="92950-283">Hodnota 64-bit, vyjádřené jako koordinovaný světový čas (UTC).</span><span class="sxs-lookup"><span data-stu-id="92950-283">A 64-bit value expressed as Coordinated Universal Time (UTC).</span></span> <span data-ttu-id="92950-284">Hello podporované rozsah začíná na 12:00 hodin, 1, 1601. ledna data a času</span><span class="sxs-lookup"><span data-stu-id="92950-284">hello supported DateTime range begins from 12:00 midnight, January 1, 1601 A.D.</span></span> <span data-ttu-id="92950-285">(C.E.), UTC.</span><span class="sxs-lookup"><span data-stu-id="92950-285">(C.E.), UTC.</span></span> <span data-ttu-id="92950-286">rozsah Hello končí u 31. prosince 9999.</span><span class="sxs-lookup"><span data-stu-id="92950-286">hello range ends at December 31, 9999.</span></span> |
| <span data-ttu-id="92950-287">Edm.Double</span><span class="sxs-lookup"><span data-stu-id="92950-287">Edm.Double</span></span> |<span data-ttu-id="92950-288">Double</span><span class="sxs-lookup"><span data-stu-id="92950-288">double</span></span> |<span data-ttu-id="92950-289">Hodnota 64-bit plovoucí bodu.</span><span class="sxs-lookup"><span data-stu-id="92950-289">A 64-bit floating point value.</span></span> |
| <span data-ttu-id="92950-290">Edm.Guid</span><span class="sxs-lookup"><span data-stu-id="92950-290">Edm.Guid</span></span> |<span data-ttu-id="92950-291">Identifikátor GUID</span><span class="sxs-lookup"><span data-stu-id="92950-291">Guid</span></span> |<span data-ttu-id="92950-292">Globálně jedinečný identifikátor 128-bit.</span><span class="sxs-lookup"><span data-stu-id="92950-292">A 128-bit globally unique identifier.</span></span> |
| <span data-ttu-id="92950-293">Edm.Int32</span><span class="sxs-lookup"><span data-stu-id="92950-293">Edm.Int32</span></span> |<span data-ttu-id="92950-294">Int32</span><span class="sxs-lookup"><span data-stu-id="92950-294">Int32</span></span> |<span data-ttu-id="92950-295">32bitové celé číslo.</span><span class="sxs-lookup"><span data-stu-id="92950-295">A 32-bit integer.</span></span> |
| <span data-ttu-id="92950-296">Edm.Int64</span><span class="sxs-lookup"><span data-stu-id="92950-296">Edm.Int64</span></span> |<span data-ttu-id="92950-297">Int64</span><span class="sxs-lookup"><span data-stu-id="92950-297">Int64</span></span> |<span data-ttu-id="92950-298">64bitové celé číslo.</span><span class="sxs-lookup"><span data-stu-id="92950-298">A 64-bit integer.</span></span> |
| <span data-ttu-id="92950-299">Edm.String</span><span class="sxs-lookup"><span data-stu-id="92950-299">Edm.String</span></span> |<span data-ttu-id="92950-300">Řetězec</span><span class="sxs-lookup"><span data-stu-id="92950-300">String</span></span> |<span data-ttu-id="92950-301">Hodnota kódování UTF-16.</span><span class="sxs-lookup"><span data-stu-id="92950-301">A UTF-16-encoded value.</span></span> <span data-ttu-id="92950-302">Řetězcové hodnoty může mít až too64 KB.</span><span class="sxs-lookup"><span data-stu-id="92950-302">String values may be up too64 KB.</span></span> |

### <a name="type-conversion-sample"></a><span data-ttu-id="92950-303">Ukázka převod typu</span><span class="sxs-lookup"><span data-stu-id="92950-303">Type Conversion Sample</span></span>
<span data-ttu-id="92950-304">Následující ukázka Hello je pro kopírování dat z Azure Blob tooAzure tabulky s převody typů.</span><span class="sxs-lookup"><span data-stu-id="92950-304">hello following sample is for copying data from an Azure Blob tooAzure Table with type conversions.</span></span>

<span data-ttu-id="92950-305">Předpokládejme, že datovou sadu objektu Blob hello je ve formátu CSV a obsahuje tři sloupce.</span><span class="sxs-lookup"><span data-stu-id="92950-305">Suppose hello Blob dataset is in CSV format and contains three columns.</span></span> <span data-ttu-id="92950-306">Jeden z nich je sloupec Datum a čas ve formátu data a času vlastní pomocí zkrácené francouzštině názvy den v týdnu hello.</span><span class="sxs-lookup"><span data-stu-id="92950-306">One of them is a datetime column with a custom datetime format using abbreviated French names for day of hello week.</span></span>

<span data-ttu-id="92950-307">Definujte datovou sadu objektu Blob zdroj hello takto společně s definic typů pro sloupce hello.</span><span class="sxs-lookup"><span data-stu-id="92950-307">Define hello Blob Source dataset as follows along with type definitions for hello columns.</span></span>

```JSON
{
    "name": " AzureBlobInput",
    "properties":
    {
         "structure":
          [
                { "name": "userid", "type": "Int64"},
                { "name": "name", "type": "String"},
                { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
          ],
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder",
            "fileName":"myfile.csv",
            "format":
            {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "external": true,
        "availability":
        {
            "frequency": "Hour",
            "interval": 1,
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
<span data-ttu-id="92950-308">Zadané mapování typu hello z typu too.NET typu OData tabulky Azure, by definovat hello tabulky v tabulce Azure s hello následující schéma.</span><span class="sxs-lookup"><span data-stu-id="92950-308">Given hello type mapping from Azure Table OData type too.NET type, you would define hello table in Azure Table with hello following schema.</span></span>

<span data-ttu-id="92950-309">**Schéma tabulky Azure:**</span><span class="sxs-lookup"><span data-stu-id="92950-309">**Azure Table schema:**</span></span>

| <span data-ttu-id="92950-310">Název sloupce</span><span class="sxs-lookup"><span data-stu-id="92950-310">Column name</span></span> | <span data-ttu-id="92950-311">Typ</span><span class="sxs-lookup"><span data-stu-id="92950-311">Type</span></span> |
| --- | --- |
| <span data-ttu-id="92950-312">ID uživatele</span><span class="sxs-lookup"><span data-stu-id="92950-312">userid</span></span> |<span data-ttu-id="92950-313">Edm.Int64</span><span class="sxs-lookup"><span data-stu-id="92950-313">Edm.Int64</span></span> |
| <span data-ttu-id="92950-314">jméno</span><span class="sxs-lookup"><span data-stu-id="92950-314">name</span></span> |<span data-ttu-id="92950-315">Edm.String</span><span class="sxs-lookup"><span data-stu-id="92950-315">Edm.String</span></span> |
| <span data-ttu-id="92950-316">lastlogindate</span><span class="sxs-lookup"><span data-stu-id="92950-316">lastlogindate</span></span> |<span data-ttu-id="92950-317">Edm.DateTime</span><span class="sxs-lookup"><span data-stu-id="92950-317">Edm.DateTime</span></span> |

<span data-ttu-id="92950-318">V dalším kroku definujte datová sada Azure Table hello následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="92950-318">Next, define hello Azure Table dataset as follows.</span></span> <span data-ttu-id="92950-319">Vzhledem k tomu, že informace o typu hello se už vyskytuje v hello základní úložiště dat nemusí toospecify "struktura" oddíl s informací o typu hello.</span><span class="sxs-lookup"><span data-stu-id="92950-319">You do not need toospecify “structure” section with hello type information since hello type information is already specified in hello underlying data store.</span></span>

```JSON
{
  "name": "AzureTableOutput",
  "properties": {
    "type": "AzureTable",
    "linkedServiceName": "StorageLinkedService",
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

<span data-ttu-id="92950-320">V takovém případě Data Factory automaticky převody typu včetně hello pole data a času ve formátu data a času vlastní hello při přesouvání dat z objektu Blob tooAzure tabulce s využitím jazykové verze hello "fr-fr".</span><span class="sxs-lookup"><span data-stu-id="92950-320">In this case, Data Factory automatically does type conversions including hello Datetime field with hello custom datetime format using hello "fr-fr" culture when moving data from Blob tooAzure Table.</span></span>

> [!NOTE]
> <span data-ttu-id="92950-321">toomap sloupce z toocolumns datové sady zdroje z podřízený datové sady, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="92950-321">toomap columns from source dataset toocolumns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="92950-322">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="92950-322">Performance and Tuning</span></span>
<span data-ttu-id="92950-323">výkon této dopad přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize faktory toolearn o klíči, najdete v části [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md).</span><span class="sxs-lookup"><span data-stu-id="92950-323">toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it, see [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md).</span></span>
