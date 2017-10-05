---
title: "Kopírování dat do/z Azure SQL Data Warehouse | Microsoft Docs"
description: "Zjistěte, jak ke zkopírování dat z Azure SQL Data Warehouse pomocí Azure Data Factory"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: d90fa9bd-4b79-458a-8d40-e896835cfd4a
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: 8cba89e0947646b498af07aa484511bf07bf7b0e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="copy-data-to-and-from-azure-sql-data-warehouse-using-azure-data-factory"></a><span data-ttu-id="4bc2b-103">Kopírování dat do a z Azure SQL Data Warehouse pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="4bc2b-103">Copy data to and from Azure SQL Data Warehouse using Azure Data Factory</span></span>
<span data-ttu-id="4bc2b-104">Tento článek vysvětluje, jak pomocí aktivity kopírování v Azure Data Factory pro přesun dat z Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-104">This article explains how to use the Copy Activity in Azure Data Factory to move data to/from Azure SQL Data Warehouse.</span></span> <span data-ttu-id="4bc2b-105">Vychází [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>  

> [!TIP]
> <span data-ttu-id="4bc2b-106">K dosažení nejlepšího výkonu, použijte k načtení dat do Azure SQL Data Warehouse PolyBase.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-106">To achieve best performance, use PolyBase to load data into Azure SQL Data Warehouse.</span></span> <span data-ttu-id="4bc2b-107">[PolyBase používá k načtení dat do Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) obsahuje podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-107">The [Use PolyBase to load data into Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) section has details.</span></span> <span data-ttu-id="4bc2b-108">Návod s případu použití najdete v tématu [načíst 1 TB do Azure SQL Data Warehouse pomocí Azure Data Factory v části 15 minut](data-factory-load-sql-data-warehouse.md).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-108">For a walkthrough with a use case, see [Load 1 TB into Azure SQL Data Warehouse under 15 minutes with Azure Data Factory](data-factory-load-sql-data-warehouse.md).</span></span>

## <a name="supported-scenarios"></a><span data-ttu-id="4bc2b-109">Podporované scénáře</span><span class="sxs-lookup"><span data-stu-id="4bc2b-109">Supported scenarios</span></span>
<span data-ttu-id="4bc2b-110">Může kopírovat data **z Azure SQL Data Warehouse** ukládá do následující data:</span><span class="sxs-lookup"><span data-stu-id="4bc2b-110">You can copy data **from Azure SQL Data Warehouse** to the following data stores:</span></span>

[!INCLUDE [data-factory-supported-sinks](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="4bc2b-111">Může kopírovat data z následujících datových úložišť **do Azure SQL Data Warehouse**:</span><span class="sxs-lookup"><span data-stu-id="4bc2b-111">You can copy data from the following data stores **to Azure SQL Data Warehouse**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

> [!TIP]
> <span data-ttu-id="4bc2b-112">Při kopírování dat z SQL serveru nebo databáze SQL Azure do Azure SQL Data Warehouse, pokud tabulka neexistuje v cílové úložiště, Data Factory můžete automaticky vytvořit v tabulce v SQL Data Warehouse pomocí schématu tabulky v úložišti dat zdroje.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-112">When copying data from SQL Server or Azure SQL Database to Azure SQL Data Warehouse, if the table does not exist in the destination store, Data Factory can automatically create the table in SQL Data Warehouse by using the schema of the table in the source data store.</span></span> <span data-ttu-id="4bc2b-113">V tématu [automatické vytvoření tabulky](#auto-table-creation) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-113">See [Auto table creation](#auto-table-creation) for details.</span></span>

## <a name="supported-authentication-type"></a><span data-ttu-id="4bc2b-114">Podporované typ ověřování</span><span class="sxs-lookup"><span data-stu-id="4bc2b-114">Supported authentication type</span></span>
<span data-ttu-id="4bc2b-115">Azure SQL Data Warehouse konektor podporu základní ověřování.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-115">Azure SQL Data Warehouse connector support basic authentication.</span></span>

## <a name="getting-started"></a><span data-ttu-id="4bc2b-116">Začínáme</span><span class="sxs-lookup"><span data-stu-id="4bc2b-116">Getting started</span></span>
<span data-ttu-id="4bc2b-117">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z Azure SQL Data Warehouse pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-117">You can create a pipeline with a copy activity that moves data to/from an Azure SQL Data Warehouse by using different tools/APIs.</span></span>

<span data-ttu-id="4bc2b-118">Nejjednodušší způsob, jak vytvořit kanál, který kopíruje data z Azure SQL Data Warehouse je pomocí Průvodce kopírováním data.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-118">The easiest way to create a pipeline that copies data to/from Azure SQL Data Warehouse is to use the Copy data wizard.</span></span> <span data-ttu-id="4bc2b-119">V tématu [kurz: načtení dat do SQL Data Warehouse pomocí služby Data Factory](../sql-data-warehouse/sql-data-warehouse-load-with-data-factory.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírováním data.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-119">See [Tutorial: Load data into SQL Data Warehouse with Data Factory](../sql-data-warehouse/sql-data-warehouse-load-with-data-factory.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="4bc2b-120">Tyto nástroje můžete také použít k vytvoření kanálu: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru**, **.NET API**, a **REST API**.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-120">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="4bc2b-121">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-121">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span>

<span data-ttu-id="4bc2b-122">Jestli používáte nástroje nebo rozhraní API, je třeba provést následující kroky k vytvoření kanálu, který přesouvá data ze zdrojového úložiště dat do úložiště dat podřízený:</span><span class="sxs-lookup"><span data-stu-id="4bc2b-122">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="4bc2b-123">Vytvoření **objekt pro vytváření dat**.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-123">Create a **data factory**.</span></span> <span data-ttu-id="4bc2b-124">Objekt pro vytváření dat může obsahovat jeden nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-124">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="4bc2b-125">Vytvoření **propojené služby** propojení vstupní a výstupní data ukládá do data factory.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-125">Create **linked services** to link input and output data stores to your data factory.</span></span> <span data-ttu-id="4bc2b-126">Pokud jsou kopírování dat z Azure blob storage do služby Azure SQL data warehouse, například vytvoříte dvě propojené služby pro vytváření dat. propojte vašeho účtu úložiště Azure a Azure SQL data warehouse.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-126">For example, if you are copying data from an Azure blob storage to an Azure SQL data warehouse, you create two linked services to link your Azure storage account and Azure SQL data warehouse to your data factory.</span></span> <span data-ttu-id="4bc2b-127">Vlastnosti propojené služby, které jsou specifické pro Azure SQL Data Warehouse, najdete v části [propojené vlastnosti služby](#linked-service-properties) části.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-127">For linked service properties that are specific to Azure SQL Data Warehouse, see [linked service properties](#linked-service-properties) section.</span></span> 
3. <span data-ttu-id="4bc2b-128">Vytvoření **datové sady** představují vstupní a výstupní data pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-128">Create **datasets** to represent input and output data for the copy operation.</span></span> <span data-ttu-id="4bc2b-129">V příkladu uvedených v posledním kroku vytvoříte datové sady a zadat kontejner objektů blob a složky, která obsahuje vstupní data.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-129">In the example mentioned in the last step, you create a dataset to specify the blob container and folder that contains the input data.</span></span> <span data-ttu-id="4bc2b-130">A vytvořte jinou datovou sadu, která zadejte v tabulce v datovém skladu Azure SQL, který obsahuje data zkopírovat z úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-130">And, you create another dataset to specify the table in the Azure SQL data warehouse that holds the data copied from the blob storage.</span></span> <span data-ttu-id="4bc2b-131">Vlastnosti datové sady, které jsou specifické pro Azure SQL Data Warehouse, najdete v části [vlastnosti datové sady](#dataset-properties) části.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-131">For dataset properties that are specific to Azure SQL Data Warehouse, see [dataset properties](#dataset-properties) section.</span></span>
4. <span data-ttu-id="4bc2b-132">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-132">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="4bc2b-133">V příkladu již bylo zmíněno dříve použijete BlobSource jako zdroj a SqlDWSink jako jímku pro aktivitu kopírování.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-133">In the example mentioned earlier, you use BlobSource as a source and SqlDWSink as a sink for the copy activity.</span></span> <span data-ttu-id="4bc2b-134">Podobně pokud kopírujete z Azure SQL Data Warehouse do úložiště objektů Blob Azure, můžete použít SqlDWSource a BlobSink v aktivitě kopírování.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-134">Similarly, if you are copying from Azure SQL Data Warehouse to Azure Blob Storage, you use SqlDWSource and BlobSink in the copy activity.</span></span> <span data-ttu-id="4bc2b-135">Kopírovat vlastnosti aktivity, které jsou specifické pro Azure SQL Data Warehouse, najdete v části [zkopírovat vlastnosti aktivity](#copy-activity-properties) části.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-135">For copy activity properties that are specific to Azure SQL Data Warehouse, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="4bc2b-136">Podrobnosti o tom, jak používat úložiště dat jako zdroj nebo jímka klikněte na odkaz v předchozí části pro data store.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-136">For details on how to use a data store as a source or a sink, click the link in the previous section for your data store.</span></span>

<span data-ttu-id="4bc2b-137">Když použijete průvodce, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-137">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="4bc2b-138">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), definujete tyto entity služby Data Factory pomocí formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-138">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="4bc2b-139">Ukázky s definicemi JSON entit služby Data Factory, které se používají ke zkopírování dat do/z Azure SQL Data Warehouse, najdete v části [JSON příklady](#json-examples-for-copying-data-to-and-from-sql-data-warehouse) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-139">For samples with JSON definitions for Data Factory entities that are used to copy data to/from an Azure SQL Data Warehouse, see [JSON examples](#json-examples-for-copying-data-to-and-from-sql-data-warehouse) section of this article.</span></span>

<span data-ttu-id="4bc2b-140">Následující části obsahují podrobnosti o vlastnostech formátu JSON, které slouží k definování konkrétní entity služby Data Factory pro Azure SQL Data Warehouse:</span><span class="sxs-lookup"><span data-stu-id="4bc2b-140">The following sections provide details about JSON properties that are used to define Data Factory entities specific to Azure SQL Data Warehouse:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="4bc2b-141">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="4bc2b-141">Linked service properties</span></span>
<span data-ttu-id="4bc2b-142">Následující tabulka obsahuje popis specifické pro službu Azure SQL Data Warehouse propojené elementy JSON.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-142">The following table provides description for JSON elements specific to Azure SQL Data Warehouse linked service.</span></span>

| <span data-ttu-id="4bc2b-143">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="4bc2b-143">Property</span></span> | <span data-ttu-id="4bc2b-144">Popis</span><span class="sxs-lookup"><span data-stu-id="4bc2b-144">Description</span></span> | <span data-ttu-id="4bc2b-145">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="4bc2b-145">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="4bc2b-146">type</span><span class="sxs-lookup"><span data-stu-id="4bc2b-146">type</span></span> |<span data-ttu-id="4bc2b-147">Vlastnost typu musí být nastavena na: **AzureSqlDW**</span><span class="sxs-lookup"><span data-stu-id="4bc2b-147">The type property must be set to: **AzureSqlDW**</span></span> |<span data-ttu-id="4bc2b-148">Ano</span><span class="sxs-lookup"><span data-stu-id="4bc2b-148">Yes</span></span> |
| <span data-ttu-id="4bc2b-149">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="4bc2b-149">connectionString</span></span> |<span data-ttu-id="4bc2b-150">Zadejte informace potřebné pro připojení k Azure SQL Data Warehouse instance pro vlastnost connectionString.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-150">Specify information needed to connect to the Azure SQL Data Warehouse instance for the connectionString property.</span></span> <span data-ttu-id="4bc2b-151">Podporováno je pouze základní ověřování.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-151">Only basic authentication is supported.</span></span> |<span data-ttu-id="4bc2b-152">Ano</span><span class="sxs-lookup"><span data-stu-id="4bc2b-152">Yes</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="4bc2b-153">Konfigurace [Firewall databáze Azure SQL](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) a databázový server, který [povolit službám Azure přístup k serveru](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-153">Configure [Azure SQL Database Firewall](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) and the database server to [allow Azure Services to access the server](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure).</span></span> <span data-ttu-id="4bc2b-154">Kromě toho pokud data kopírujete do Azure SQL Data Warehouse z mimo Azure včetně z místního zdroje dat se brána objekt pro vytváření dat, nakonfigurujte příslušné rozsah IP adres pro počítač, který odesílá data do Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-154">Additionally, if you are copying data to Azure SQL Data Warehouse from outside Azure including from on-premises data sources with data factory gateway, configure appropriate IP address range for the machine that is sending data to Azure SQL Data Warehouse.</span></span>

## <a name="dataset-properties"></a><span data-ttu-id="4bc2b-155">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="4bc2b-155">Dataset properties</span></span>
<span data-ttu-id="4bc2b-156">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v článku [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-156">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="4bc2b-157">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-157">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="4bc2b-158">V rámci typeProperties části se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění dat v úložišti.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-158">The typeProperties section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="4bc2b-159">**Rámci typeProperties** části datové sady typu **AzureSqlDWTable** má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="4bc2b-159">The **typeProperties** section for the dataset of type **AzureSqlDWTable** has the following properties:</span></span>

| <span data-ttu-id="4bc2b-160">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="4bc2b-160">Property</span></span> | <span data-ttu-id="4bc2b-161">Popis</span><span class="sxs-lookup"><span data-stu-id="4bc2b-161">Description</span></span> | <span data-ttu-id="4bc2b-162">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="4bc2b-162">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="4bc2b-163">tableName</span><span class="sxs-lookup"><span data-stu-id="4bc2b-163">tableName</span></span> |<span data-ttu-id="4bc2b-164">Název tabulky nebo zobrazení v databázi Azure SQL Data Warehouse, která propojená služba odkazuje.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-164">Name of the table or view in the Azure SQL Data Warehouse database that the linked service refers to.</span></span> |<span data-ttu-id="4bc2b-165">Ano</span><span class="sxs-lookup"><span data-stu-id="4bc2b-165">Yes</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="4bc2b-166">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="4bc2b-166">Copy activity properties</span></span>
<span data-ttu-id="4bc2b-167">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-167">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="4bc2b-168">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-168">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

> [!NOTE]
> <span data-ttu-id="4bc2b-169">Aktivita kopírování přijímá pouze jeden vstup a vytváří jenom jeden výstup.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-169">The Copy Activity takes only one input and produces only one output.</span></span>

<span data-ttu-id="4bc2b-170">Vzhledem k tomu, vlastnosti dostupné v rámci typeProperties části aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-170">Whereas, properties available in the typeProperties section of the activity vary with each activity type.</span></span> <span data-ttu-id="4bc2b-171">Pro aktivitu kopírování budou lišit v závislosti na typech zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-171">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

### <a name="sqldwsource"></a><span data-ttu-id="4bc2b-172">SqlDWSource</span><span class="sxs-lookup"><span data-stu-id="4bc2b-172">SqlDWSource</span></span>
<span data-ttu-id="4bc2b-173">Pokud je zdroj typu **SqlDWSource**, následující vlastnosti jsou k dispozici v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="4bc2b-173">When source is of type **SqlDWSource**, the following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="4bc2b-174">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="4bc2b-174">Property</span></span> | <span data-ttu-id="4bc2b-175">Popis</span><span class="sxs-lookup"><span data-stu-id="4bc2b-175">Description</span></span> | <span data-ttu-id="4bc2b-176">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="4bc2b-176">Allowed values</span></span> | <span data-ttu-id="4bc2b-177">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="4bc2b-177">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="4bc2b-178">sqlReaderQuery</span><span class="sxs-lookup"><span data-stu-id="4bc2b-178">sqlReaderQuery</span></span> |<span data-ttu-id="4bc2b-179">Čtení dat pomocí vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-179">Use the custom query to read data.</span></span> |<span data-ttu-id="4bc2b-180">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-180">SQL query string.</span></span> <span data-ttu-id="4bc2b-181">Příklad: vybrat * z MyTable.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-181">For example: select * from MyTable.</span></span> |<span data-ttu-id="4bc2b-182">Ne</span><span class="sxs-lookup"><span data-stu-id="4bc2b-182">No</span></span> |
| <span data-ttu-id="4bc2b-183">sqlReaderStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="4bc2b-183">sqlReaderStoredProcedureName</span></span> |<span data-ttu-id="4bc2b-184">Název uložené procedury, který čte data ze zdrojové tabulky.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-184">Name of the stored procedure that reads data from the source table.</span></span> |<span data-ttu-id="4bc2b-185">Název uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-185">Name of the stored procedure.</span></span> <span data-ttu-id="4bc2b-186">Poslední příkaz jazyka SQL musí být příkaz SELECT v uložené proceduře.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-186">The last SQL statement must be a SELECT statement in the stored procedure.</span></span> |<span data-ttu-id="4bc2b-187">Ne</span><span class="sxs-lookup"><span data-stu-id="4bc2b-187">No</span></span> |
| <span data-ttu-id="4bc2b-188">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="4bc2b-188">storedProcedureParameters</span></span> |<span data-ttu-id="4bc2b-189">Parametry pro uloženou proceduru.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-189">Parameters for the stored procedure.</span></span> |<span data-ttu-id="4bc2b-190">Páry název/hodnota.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-190">Name/value pairs.</span></span> <span data-ttu-id="4bc2b-191">Názvy a malá a velká písmena parametry musí odpovídat názvům a malá a velká písmena parametry uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-191">Names and casing of parameters must match the names and casing of the stored procedure parameters.</span></span> |<span data-ttu-id="4bc2b-192">Ne</span><span class="sxs-lookup"><span data-stu-id="4bc2b-192">No</span></span> |

<span data-ttu-id="4bc2b-193">Pokud **sqlReaderQuery** je zadán pro SqlDWSource, spuštění aktivity kopírování tohoto dotazu na zdroji Azure SQL Data Warehouse se získat data.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-193">If the **sqlReaderQuery** is specified for the SqlDWSource, the Copy Activity runs this query against the Azure SQL Data Warehouse source to get the data.</span></span>

<span data-ttu-id="4bc2b-194">Alternativně můžete zadat uložené procedury zadáním **sqlReaderStoredProcedureName** a **storedProcedureParameters** (Pokud uložená procedura přebírá parametry).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-194">Alternatively, you can specify a stored procedure by specifying the **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if the stored procedure takes parameters).</span></span>

<span data-ttu-id="4bc2b-195">Pokud nezadáte sqlReaderQuery nebo sqlReaderStoredProcedureName, sloupce definované v části Struktura sady dat JSON se používají k vytvoření dotazu ke spouštění na Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-195">If you do not specify either sqlReaderQuery or sqlReaderStoredProcedureName, the columns defined in the structure section of the dataset JSON are used to build a query to run against the Azure SQL Data Warehouse.</span></span> <span data-ttu-id="4bc2b-196">Příklad: `select column1, column2 from mytable`.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-196">Example: `select column1, column2 from mytable`.</span></span> <span data-ttu-id="4bc2b-197">Pokud definice datové sady nemá strukturu, jsou vybrány všechny sloupce z tabulky.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-197">If the dataset definition does not have the structure, all columns are selected from the table.</span></span>

#### <a name="sqldwsource-example"></a><span data-ttu-id="4bc2b-198">Příklad SqlDWSource</span><span class="sxs-lookup"><span data-stu-id="4bc2b-198">SqlDWSource example</span></span>

```JSON
"source": {
    "type": "SqlDWSource",
    "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
    "storedProcedureParameters": {
        "stringData": { "value": "str3" },
        "identifier": { "value": "$$Text.Format('{0:yyyy}', SliceStart)", "type": "Int"}
    }
}
```
<span data-ttu-id="4bc2b-199">**Uložená procedura definice:**</span><span class="sxs-lookup"><span data-stu-id="4bc2b-199">**The stored procedure definition:**</span></span>

```SQL
CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
(
    @stringData varchar(20),
    @identifier int
)
AS
SET NOCOUNT ON;
BEGIN
     select *
     from dbo.UnitTestSrcTable
     where dbo.UnitTestSrcTable.stringData != stringData
    and dbo.UnitTestSrcTable.identifier != identifier
END
GO
```

### <a name="sqldwsink"></a><span data-ttu-id="4bc2b-200">SqlDWSink</span><span class="sxs-lookup"><span data-stu-id="4bc2b-200">SqlDWSink</span></span>
<span data-ttu-id="4bc2b-201">**SqlDWSink** podporuje následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="4bc2b-201">**SqlDWSink** supports the following properties:</span></span>

| <span data-ttu-id="4bc2b-202">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="4bc2b-202">Property</span></span> | <span data-ttu-id="4bc2b-203">Popis</span><span class="sxs-lookup"><span data-stu-id="4bc2b-203">Description</span></span> | <span data-ttu-id="4bc2b-204">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="4bc2b-204">Allowed values</span></span> | <span data-ttu-id="4bc2b-205">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="4bc2b-205">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="4bc2b-206">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="4bc2b-206">sqlWriterCleanupScript</span></span> |<span data-ttu-id="4bc2b-207">Zadejte dotaz pro aktivitu kopírování provést tak, aby se vyčistit data určitý řez.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-207">Specify a query for Copy Activity to execute such that data of a specific slice is cleaned up.</span></span> <span data-ttu-id="4bc2b-208">Podrobnosti najdete v tématu [opakovatelnosti části](#repeatability-during-copy).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-208">For details, see [repeatability section](#repeatability-during-copy).</span></span> |<span data-ttu-id="4bc2b-209">Příkaz dotazu.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-209">A query statement.</span></span> |<span data-ttu-id="4bc2b-210">Ne</span><span class="sxs-lookup"><span data-stu-id="4bc2b-210">No</span></span> |
| <span data-ttu-id="4bc2b-211">allowPolyBase</span><span class="sxs-lookup"><span data-stu-id="4bc2b-211">allowPolyBase</span></span> |<span data-ttu-id="4bc2b-212">Označuje, zda místo hromadné vložení mechanismus použít PolyBase (v případě potřeby).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-212">Indicates whether to use PolyBase (when applicable) instead of BULKINSERT mechanism.</span></span> <br/><br/> <span data-ttu-id="4bc2b-213">**Pomocí PolyBase je doporučeným způsobem, jak načíst data do SQL Data Warehouse.**</span><span class="sxs-lookup"><span data-stu-id="4bc2b-213">**Using PolyBase is the recommended way to load data into SQL Data Warehouse.**</span></span> <span data-ttu-id="4bc2b-214">V tématu [PolyBase používá k načtení dat do Azure SQL Data Warehouse](#use-polybase-to-load-data-into-azure-sql-data-warehouse) části omezení a podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-214">See [Use PolyBase to load data into Azure SQL Data Warehouse](#use-polybase-to-load-data-into-azure-sql-data-warehouse) section for constraints and details.</span></span> |<span data-ttu-id="4bc2b-215">True</span><span class="sxs-lookup"><span data-stu-id="4bc2b-215">True</span></span> <br/><span data-ttu-id="4bc2b-216">NEPRAVDA (výchozí)</span><span class="sxs-lookup"><span data-stu-id="4bc2b-216">False (default)</span></span> |<span data-ttu-id="4bc2b-217">Ne</span><span class="sxs-lookup"><span data-stu-id="4bc2b-217">No</span></span> |
| <span data-ttu-id="4bc2b-218">polyBaseSettings</span><span class="sxs-lookup"><span data-stu-id="4bc2b-218">polyBaseSettings</span></span> |<span data-ttu-id="4bc2b-219">Skupinu vlastností, které se dají zadat při **allowPolybase** je nastavena na **true**.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-219">A group of properties that can be specified when the **allowPolybase** property is set to **true**.</span></span> |&nbsp; |<span data-ttu-id="4bc2b-220">Ne</span><span class="sxs-lookup"><span data-stu-id="4bc2b-220">No</span></span> |
| <span data-ttu-id="4bc2b-221">rejectValue</span><span class="sxs-lookup"><span data-stu-id="4bc2b-221">rejectValue</span></span> |<span data-ttu-id="4bc2b-222">Určuje číslo nebo podíl řádků, které může být odmítnutá předtím, než se dotaz nezdaří.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-222">Specifies the number or percentage of rows that can be rejected before the query fails.</span></span> <br/><br/><span data-ttu-id="4bc2b-223">Další informace o možnostech odmítněte používání funkce PolyBase v **argumenty** části [vytvořit EXTERNAL TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) tématu.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-223">Learn more about the PolyBase’s reject options in the **Arguments** section of [CREATE EXTERNAL TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) topic.</span></span> |<span data-ttu-id="4bc2b-224">0 (výchozí), 1, 2...</span><span class="sxs-lookup"><span data-stu-id="4bc2b-224">0 (default), 1, 2, …</span></span> |<span data-ttu-id="4bc2b-225">Ne</span><span class="sxs-lookup"><span data-stu-id="4bc2b-225">No</span></span> |
| <span data-ttu-id="4bc2b-226">rejectType</span><span class="sxs-lookup"><span data-stu-id="4bc2b-226">rejectType</span></span> |<span data-ttu-id="4bc2b-227">Určuje, zda je hodnota literálu nebo jako procento zadána možnost rejectValue.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-227">Specifies whether the rejectValue option is specified as a literal value or a percentage.</span></span> |<span data-ttu-id="4bc2b-228">Hodnota (výchozí), procento</span><span class="sxs-lookup"><span data-stu-id="4bc2b-228">Value (default), Percentage</span></span> |<span data-ttu-id="4bc2b-229">Ne</span><span class="sxs-lookup"><span data-stu-id="4bc2b-229">No</span></span> |
| <span data-ttu-id="4bc2b-230">rejectSampleValue</span><span class="sxs-lookup"><span data-stu-id="4bc2b-230">rejectSampleValue</span></span> |<span data-ttu-id="4bc2b-231">Určuje počet řádků k načtení předtím, než PolyBase přepočítá procento odmítnutých řádků.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-231">Determines the number of rows to retrieve before the PolyBase recalculates the percentage of rejected rows.</span></span> |<span data-ttu-id="4bc2b-232">1, 2, …</span><span class="sxs-lookup"><span data-stu-id="4bc2b-232">1, 2, …</span></span> |<span data-ttu-id="4bc2b-233">Ano, pokud **rejectType** je **procento**</span><span class="sxs-lookup"><span data-stu-id="4bc2b-233">Yes, if **rejectType** is **percentage**</span></span> |
| <span data-ttu-id="4bc2b-234">useTypeDefault</span><span class="sxs-lookup"><span data-stu-id="4bc2b-234">useTypeDefault</span></span> |<span data-ttu-id="4bc2b-235">Určuje způsob zpracování chybějící hodnoty v textových souborů s oddělovači, když PolyBase načítá data z textového souboru.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-235">Specifies how to handle missing values in delimited text files when PolyBase retrieves data from the text file.</span></span><br/><br/><span data-ttu-id="4bc2b-236">Další informace o této vlastnosti v části argumenty [vytvořit EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-236">Learn more about this property from the Arguments section in [CREATE EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx).</span></span> |<span data-ttu-id="4bc2b-237">Hodnota TRUE, False (výchozí)</span><span class="sxs-lookup"><span data-stu-id="4bc2b-237">True, False (default)</span></span> |<span data-ttu-id="4bc2b-238">Ne</span><span class="sxs-lookup"><span data-stu-id="4bc2b-238">No</span></span> |
| <span data-ttu-id="4bc2b-239">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="4bc2b-239">writeBatchSize</span></span> |<span data-ttu-id="4bc2b-240">Vloží data do tabulky SQL, když velikost vyrovnávací paměti dosáhne writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="4bc2b-240">Inserts data into the SQL table when the buffer size reaches writeBatchSize</span></span> |<span data-ttu-id="4bc2b-241">Celé číslo (počet řádků)</span><span class="sxs-lookup"><span data-stu-id="4bc2b-241">Integer (number of rows)</span></span> |<span data-ttu-id="4bc2b-242">Ne (výchozí: 10000)</span><span class="sxs-lookup"><span data-stu-id="4bc2b-242">No (default: 10000)</span></span> |
| <span data-ttu-id="4bc2b-243">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="4bc2b-243">writeBatchTimeout</span></span> |<span data-ttu-id="4bc2b-244">Počkejte, než čas na dokončení předtím, než vyprší časový limit operace dávkové vložení.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-244">Wait time for the batch insert operation to complete before it times out.</span></span> |<span data-ttu-id="4bc2b-245">Časový interval</span><span class="sxs-lookup"><span data-stu-id="4bc2b-245">timespan</span></span><br/><br/> <span data-ttu-id="4bc2b-246">Příklad: "00: 30:00" (30 minut).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-246">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="4bc2b-247">Ne</span><span class="sxs-lookup"><span data-stu-id="4bc2b-247">No</span></span> |

#### <a name="sqldwsink-example"></a><span data-ttu-id="4bc2b-248">Příklad SqlDWSink</span><span class="sxs-lookup"><span data-stu-id="4bc2b-248">SqlDWSink example</span></span>

```JSON
"sink": {
    "type": "SqlDWSink",
    "allowPolyBase": true
}
```

## <a name="use-polybase-to-load-data-into-azure-sql-data-warehouse"></a><span data-ttu-id="4bc2b-249">Použijte PolyBase k načtení dat do Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="4bc2b-249">Use PolyBase to load data into Azure SQL Data Warehouse</span></span>
<span data-ttu-id="4bc2b-250">Pomocí  **[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide)**  je účinný způsob načítání velké množství dat do Azure SQL Data Warehouse s vysokou propustností.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-250">Using **[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide)** is an efficient way of loading large amount of data into Azure SQL Data Warehouse with high throughput.</span></span> <span data-ttu-id="4bc2b-251">Velký nárůst v propustnost můžete zobrazit pomocí PolyBase místo výchozího mechanismu hromadné vložení.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-251">You can see a large gain in the throughput by using PolyBase instead of the default BULKINSERT mechanism.</span></span> <span data-ttu-id="4bc2b-252">V tématu [zkopírujte výkonu referenční číslo](data-factory-copy-activity-performance.md#performance-reference) s podrobné porovnání.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-252">See [copy performance reference number](data-factory-copy-activity-performance.md#performance-reference) with detailed comparison.</span></span> <span data-ttu-id="4bc2b-253">Návod s případu použití najdete v tématu [načíst 1 TB do Azure SQL Data Warehouse pomocí Azure Data Factory v části 15 minut](data-factory-load-sql-data-warehouse.md).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-253">For a walkthrough with a use case, see [Load 1 TB into Azure SQL Data Warehouse under 15 minutes with Azure Data Factory](data-factory-load-sql-data-warehouse.md).</span></span>

* <span data-ttu-id="4bc2b-254">Pokud je zdroj dat v **objektů Blob v Azure nebo Azure Data Lake Store**a formát je kompatibilní s funkcí PolyBase, můžete zkopírovat přímo do Azure SQL Data Warehouse pomocí PolyBase.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-254">If your source data is in **Azure Blob or Azure Data Lake Store**, and the format is compatible with PolyBase, you can directly copy to Azure SQL Data Warehouse using PolyBase.</span></span> <span data-ttu-id="4bc2b-255">V tématu  **[přímé kopírování pomocí PolyBase](#direct-copy-using-polybase)**  s podrobnostmi.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-255">See **[Direct copy using PolyBase](#direct-copy-using-polybase)** with details.</span></span>
* <span data-ttu-id="4bc2b-256">Pokud vaše zdrojového úložiště dat a formát není podporován původně polybase, můžete použít  **[připravený kopírování pomocí PolyBase](#staged-copy-using-polybase)**  místo toho funkci.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-256">If your source data store and format is not originally supported by PolyBase, you can use the **[Staged Copy using PolyBase](#staged-copy-using-polybase)** feature instead.</span></span> <span data-ttu-id="4bc2b-257">Také poskytuje lepší propustnosti automaticky převod dat do formátu kompatibilní s funkcí PolyBase a ukládání dat v úložišti objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-257">It also provides you better throughput by automatically converting the data into PolyBase-compatible format and storing the data in Azure Blob storage.</span></span> <span data-ttu-id="4bc2b-258">Potom načte data do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-258">It then loads data into SQL Data Warehouse.</span></span>

<span data-ttu-id="4bc2b-259">Nastavte `allowPolyBase` vlastnost **true** jak je znázorněno v následujícím příkladu pro Azure Data Factory ke zkopírování dat do Azure SQL Data Warehouse pomocí funkce PolyBase.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-259">Set the `allowPolyBase` property to **true** as shown in the following example for Azure Data Factory to use PolyBase to copy data into Azure SQL Data Warehouse.</span></span> <span data-ttu-id="4bc2b-260">Když nastavíte allowPolyBase na hodnotu true, můžete zadat konkrétní vlastnosti PolyBase pomocí `polyBaseSettings` skupina vlastností.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-260">When you set allowPolyBase to true, you can specify PolyBase specific properties using the `polyBaseSettings` property group.</span></span> <span data-ttu-id="4bc2b-261">najdete v článku [SqlDWSink](#SqlDWSink) část Podrobnosti o vlastnosti, které můžete použít s polyBaseSettings.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-261">see the [SqlDWSink](#SqlDWSink) section for details about properties that you can use with polyBaseSettings.</span></span>

```JSON
"sink": {
    "type": "SqlDWSink",
    "allowPolyBase": true,
    "polyBaseSettings":
    {
        "rejectType": "percentage",
        "rejectValue": 10.0,
        "rejectSampleValue": 100,
        "useTypeDefault": true
    }
}
```

### <a name="direct-copy-using-polybase"></a><span data-ttu-id="4bc2b-262">Přímé kopírování pomocí PolyBase</span><span class="sxs-lookup"><span data-stu-id="4bc2b-262">Direct copy using PolyBase</span></span>
<span data-ttu-id="4bc2b-263">SQL Data Warehouse PolyBase přímo podporují objektů Blob v Azure a Azure Data Lake Store (pomocí instanční objekt) jako zdroj a s požadavky na konkrétní soubor formátu.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-263">SQL Data Warehouse PolyBase directly support Azure Blob and Azure Data Lake Store (using service principal) as source and with specific file format requirements.</span></span> <span data-ttu-id="4bc2b-264">Pokud vaše zdrojová data splňuje kritéria popsaných v této části, můžete přímo zkopírovat ze zdrojového úložiště dat do Azure SQL Data Warehouse pomocí PolyBase.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-264">If your source data meets the criteria described in this section, you can directly copy from source data store to Azure SQL Data Warehouse using PolyBase.</span></span> <span data-ttu-id="4bc2b-265">Jinak můžete použít [připravený kopírování pomocí PolyBase](#staged-copy-using-polybase).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-265">Otherwise, you can use [Staged Copy using PolyBase](#staged-copy-using-polybase).</span></span>

> [!TIP]
> <span data-ttu-id="4bc2b-266">Ke zkopírování dat z Data Lake Store k SQL Data Warehouse efektivně, další informace z [Azure Data Factory usnadňuje i a pohodlný k odhalení přehled o datech, při použití Data Lake Store s SQL Data Warehouse](https://blogs.msdn.microsoft.com/azuredatalake/2017/04/08/azure-data-factory-makes-it-even-easier-and-convenient-to-uncover-insights-from-data-when-using-data-lake-store-with-sql-data-warehouse/).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-266">To copy data from Data Lake Store to SQL Data Warehouse efficiently, learn more from [Azure Data Factory makes it even easier and convenient to uncover insights from data when using Data Lake Store with SQL Data Warehouse](https://blogs.msdn.microsoft.com/azuredatalake/2017/04/08/azure-data-factory-makes-it-even-easier-and-convenient-to-uncover-insights-from-data-when-using-data-lake-store-with-sql-data-warehouse/).</span></span>

<span data-ttu-id="4bc2b-267">Pokud požadavky nejsou splněny, zkontroluje nastavení Azure Data Factory a automaticky se vrátí k hromadné vložení mechanismus pro přesun dat.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-267">If the requirements are not met, Azure Data Factory checks the settings and automatically falls back to the BULKINSERT mechanism for the data movement.</span></span>

1. <span data-ttu-id="4bc2b-268">**Zdroj propojené služby** je typu: **azurestorage** nebo **AzureDataLakeStore s objekt zabezpečení ověřování služby**.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-268">**Source linked service** is of type: **AzureStorage** or **AzureDataLakeStore with service principal authentication**.</span></span>  
2. <span data-ttu-id="4bc2b-269">**Vstupní datové sady** je typu: **AzureBlob** nebo **AzureDataLakeStore**a zadejte v části formát `type` vlastnosti je **OrcFormat**, nebo **TextFormat** s následující konfigurace:</span><span class="sxs-lookup"><span data-stu-id="4bc2b-269">The **input dataset** is of type: **AzureBlob** or **AzureDataLakeStore**, and the format type under `type` properties is **OrcFormat**, or **TextFormat** with the following configurations:</span></span>

   1. <span data-ttu-id="4bc2b-270">`rowDelimiter`musí být  **\n** .</span><span class="sxs-lookup"><span data-stu-id="4bc2b-270">`rowDelimiter` must be **\n**.</span></span>
   2. <span data-ttu-id="4bc2b-271">`nullValue`je nastavena na **prázdný řetězec** (""), nebo `treatEmptyAsNull` je nastaven na **true**.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-271">`nullValue` is set to **empty string** (""), or `treatEmptyAsNull` is set to **true**.</span></span>
   3. <span data-ttu-id="4bc2b-272">`encodingName`je nastavena na **znakové sady utf-8**, což je **výchozí** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-272">`encodingName` is set to **utf-8**, which is **default** value.</span></span>
   4. <span data-ttu-id="4bc2b-273">`escapeChar`, `quoteChar`, `firstRowAsHeader`, a `skipLineCount` nejsou zadané.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-273">`escapeChar`, `quoteChar`, `firstRowAsHeader`, and `skipLineCount` are not specified.</span></span>
   5. <span data-ttu-id="4bc2b-274">`compression`může být **bez komprese**, **GZip**, nebo **Deflate**.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-274">`compression` can be **no compression**, **GZip**, or **Deflate**.</span></span>

    ```JSON
    "typeProperties": {
       "folderPath": "<blobpath>",
       "format": {
           "type": "TextFormat",     
           "columnDelimiter": "<any delimiter>",
           "rowDelimiter": "\n",       
           "nullValue": "",           
           "encodingName": "utf-8"    
       },
       "compression": {  
           "type": "GZip",  
           "level": "Optimal"  
       }  
    },
    ```

3. <span data-ttu-id="4bc2b-275">Neexistuje žádné `skipHeaderLineCount` nastavení v části **BlobSource** nebo **AzureDataLakeStore** pro aktivitu kopírování v kanálu.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-275">There is no `skipHeaderLineCount` setting under **BlobSource** or **AzureDataLakeStore** for the Copy activity in the pipeline.</span></span>
4. <span data-ttu-id="4bc2b-276">Neexistuje žádné `sliceIdentifierColumnName` nastavení v části **SqlDWSink** pro aktivitu kopírování v kanálu.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-276">There is no `sliceIdentifierColumnName` setting under **SqlDWSink** for the Copy activity in the pipeline.</span></span> <span data-ttu-id="4bc2b-277">(PolyBase zaručuje, že se aktualizuje všechna data nebo nic se aktualizuje v jednom spustit.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-277">(PolyBase guarantees that all data is updated or nothing is updated in a single run.</span></span> <span data-ttu-id="4bc2b-278">K dosažení **opakovatelnosti**, můžete použít `sqlWriterCleanupScript`).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-278">To achieve **repeatability**, you could use `sqlWriterCleanupScript`).</span></span>
5. <span data-ttu-id="4bc2b-279">Neexistuje žádné `columnMapping` použitá v přidružené v kopie aktivity.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-279">There is no `columnMapping` being used in the associated in Copy activity.</span></span>

### <a name="staged-copy-using-polybase"></a><span data-ttu-id="4bc2b-280">Dvoufázové instalace kopírování pomocí PolyBase</span><span class="sxs-lookup"><span data-stu-id="4bc2b-280">Staged Copy using PolyBase</span></span>
<span data-ttu-id="4bc2b-281">Pokud vaše zdrojová data nesplňuje kritéria byla zavedená v předchozí části, můžete povolit kopírování dat prostřednictvím dočasné pracovní Azure Blob Storage (úložiště úrovně Premium nemůže být).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-281">When your source data doesn’t meet the criteria introduced in the previous section, you can enable copying data via an interim staging Azure Blob Storage (cannot be Premium Storage).</span></span> <span data-ttu-id="4bc2b-282">V takovém případě Azure Data Factory automaticky provede transformace na data, která mají požadavkům data formátu PolyBase, potom použijte PolyBase k načtení dat do SQL Data Warehouse a v poslední čištění dočasná data z úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-282">In this case, Azure Data Factory automatically performs transformations on the data to meet data format requirements of PolyBase, then use PolyBase to load data into SQL Data Warehouse, and at last clean-up your temp data from the Blob storage.</span></span> <span data-ttu-id="4bc2b-283">V tématu [připravený kopie](data-factory-copy-activity-performance.md#staged-copy) informace o tom, jak kopírování dat prostřednictvím pracovní objektů Blob v Azure funguje obecně.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-283">See [Staged Copy](data-factory-copy-activity-performance.md#staged-copy) for details on how copying data via a staging Azure Blob works in general.</span></span>

> [!NOTE]
> <span data-ttu-id="4bc2b-284">Při kopírování dat z data místní ukládání do Azure SQL Data Warehouse pomocí PolyBase a pracovní, pokud vaše verze brány pro správu dat je menší než 2.4, prostředí JRE (prostředí Java Runtime) se vyžaduje na počítači brány, který slouží k transformaci zdrojová data na správný formát.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-284">When copying data from an on-prem data store into Azure SQL Data Warehouse using PolyBase and staging, if your Data Management Gateway version is below 2.4, JRE (Java Runtime Environment) is required on your gateway machine that is used to transform your source data into proper format.</span></span> <span data-ttu-id="4bc2b-285">Naznačují, že upgrade brány na nejnovější předejdete této závislosti.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-285">Suggest you upgrade your gateway to the latest to avoid such dependency.</span></span>
>

<span data-ttu-id="4bc2b-286">Chcete-li tuto funkci používat, vytvořte [propojená služba Azure Storage](data-factory-azure-blob-connector.md#azure-storage-linked-service) který odkazuje na účet úložiště Azure, který má dočasné objektu blob úložiště, zadejte `enableStaging` a `stagingSettings` vlastnosti pro aktivitu kopírování, jak je znázorněno v následujícím kódu:</span><span class="sxs-lookup"><span data-stu-id="4bc2b-286">To use this feature, create an [Azure Storage linked service](data-factory-azure-blob-connector.md#azure-storage-linked-service) that refers to the Azure Storage Account that has the interim blob storage, then specify the `enableStaging` and `stagingSettings` properties for the Copy Activity as shown in the following code:</span></span>

```json
"activities":[  
{
    "name": "Sample copy activity from SQL Server to SQL Data Warehouse via PolyBase",
    "type": "Copy",
    "inputs": [{ "name": "OnpremisesSQLServerInput" }],
    "outputs": [{ "name": "AzureSQLDWOutput" }],
    "typeProperties": {
        "source": {
            "type": "SqlSource",
        },
        "sink": {
            "type": "SqlDwSink",
            "allowPolyBase": true
        },
        "enableStaging": true,
        "stagingSettings": {
            "linkedServiceName": "MyStagingBlob"
        }
    }
}
]
```

## <a name="best-practices-when-using-polybase"></a><span data-ttu-id="4bc2b-287">Osvědčené postupy při používání funkce PolyBase</span><span class="sxs-lookup"><span data-stu-id="4bc2b-287">Best practices when using PolyBase</span></span>
<span data-ttu-id="4bc2b-288">Následující části obsahují další osvědčené postupy pro ty, které jsou uvedené v [osvědčené postupy pro Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-288">The following sections provide additional best practices to the ones that are mentioned in [Best practices for Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md).</span></span>

### <a name="required-database-permission"></a><span data-ttu-id="4bc2b-289">Oprávnění požadovaná databáze</span><span class="sxs-lookup"><span data-stu-id="4bc2b-289">Required database permission</span></span>
<span data-ttu-id="4bc2b-290">Pokud chcete používat funkce PolyBase, vyžaduje má uživatel používaný k načtení dat do SQL Data Warehouse [oprávnění "Řízení"](https://msdn.microsoft.com/library/ms191291.aspx) na cílovou databázi.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-290">To use PolyBase, it requires the user being used to load data into SQL Data Warehouse has the ["CONTROL" permission](https://msdn.microsoft.com/library/ms191291.aspx) on the target database.</span></span> <span data-ttu-id="4bc2b-291">Jeden způsob, jak dosáhnout, který je přidejte tohoto uživatele jako člen role "db_owner".</span><span class="sxs-lookup"><span data-stu-id="4bc2b-291">One way to achieve that is to add that user as a member of "db_owner" role.</span></span> <span data-ttu-id="4bc2b-292">Zjistěte, jak to provést pomocí následujících [v této části](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md#authorization).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-292">Learn how to do that by following [this section](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md#authorization).</span></span>

### <a name="row-size-and-data-type-limitation"></a><span data-ttu-id="4bc2b-293">Velikost řádku a datový typ omezení</span><span class="sxs-lookup"><span data-stu-id="4bc2b-293">Row size and data type limitation</span></span>
<span data-ttu-id="4bc2b-294">Polybase zatížení jsou omezeny na obou načítání řádků menší než **1 MB** a nelze načíst VARCHR(MAX), NVARCHAR(MAX) nebo VARBINARY(MAX).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-294">Polybase loads are limited to loading rows both smaller than **1 MB** and cannot load to VARCHR(MAX), NVARCHAR(MAX) or VARBINARY(MAX).</span></span> <span data-ttu-id="4bc2b-295">Odkazovat na [zde](../sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md#loads).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-295">Refer to [here](../sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md#loads).</span></span>

<span data-ttu-id="4bc2b-296">Pokud máte zdroj dat s řádky o velikosti větší než 1 MB, můžete rozdělit zdrojové tabulky svisle na několik malých sítí, kde největší velikost řádku každého z nich není delší než limit.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-296">If you have source data with rows of size greater than 1 MB, you may want to split the source tables vertically into several small ones where the largest row size of each of them does not exceed the limit.</span></span> <span data-ttu-id="4bc2b-297">Menší tabulky pak lze načíst pomocí PolyBase a sloučí v Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-297">The smaller tables can then be loaded using PolyBase and merged together in Azure SQL Data Warehouse.</span></span>

### <a name="sql-data-warehouse-resource-class"></a><span data-ttu-id="4bc2b-298">Třída prostředků SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="4bc2b-298">SQL Data Warehouse resource class</span></span>
<span data-ttu-id="4bc2b-299">K dosažení nejlepší možné propustnost, zvažte, pro větší Třída prostředků přiřadit uživatele používaný k načtení dat do SQL Data Warehouse pomocí PolyBase.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-299">To achieve best possible throughput, consider to assign larger resource class to the user being used to load data into SQL Data Warehouse via PolyBase.</span></span> <span data-ttu-id="4bc2b-300">Zjistěte, jak to provést pomocí následujících [změnit v příkladu třída prostředků uživatele](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-300">Learn how to do that by following [Change a user resource class example](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).</span></span>

### <a name="tablename-in-azure-sql-data-warehouse"></a><span data-ttu-id="4bc2b-301">Název tabulky v Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="4bc2b-301">tableName in Azure SQL Data Warehouse</span></span>
<span data-ttu-id="4bc2b-302">Následující tabulka obsahuje příklady, jak můžete zadat **tableName** vlastnost v datové sadě JSON pro různé kombinace názvu schéma a tabulku.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-302">The following table provides examples on how to specify the **tableName** property in dataset JSON for various combinations of schema and table name.</span></span>

| <span data-ttu-id="4bc2b-303">Schéma databáze</span><span class="sxs-lookup"><span data-stu-id="4bc2b-303">DB Schema</span></span> | <span data-ttu-id="4bc2b-304">Název tabulky</span><span class="sxs-lookup"><span data-stu-id="4bc2b-304">Table name</span></span> | <span data-ttu-id="4bc2b-305">hodnotu vlastnosti tableName JSON</span><span class="sxs-lookup"><span data-stu-id="4bc2b-305">tableName JSON property</span></span> |
| --- | --- | --- |
| <span data-ttu-id="4bc2b-306">dbo</span><span class="sxs-lookup"><span data-stu-id="4bc2b-306">dbo</span></span> |<span data-ttu-id="4bc2b-307">MyTable</span><span class="sxs-lookup"><span data-stu-id="4bc2b-307">MyTable</span></span> |<span data-ttu-id="4bc2b-308">MyTable nebo dbo. MyTable nebo [dbo]. [Tabulka]</span><span class="sxs-lookup"><span data-stu-id="4bc2b-308">MyTable or dbo.MyTable or [dbo].[MyTable]</span></span> |
| <span data-ttu-id="4bc2b-309">dbo1</span><span class="sxs-lookup"><span data-stu-id="4bc2b-309">dbo1</span></span> |<span data-ttu-id="4bc2b-310">MyTable</span><span class="sxs-lookup"><span data-stu-id="4bc2b-310">MyTable</span></span> |<span data-ttu-id="4bc2b-311">dbo1. MyTable nebo [dbo1]. [Tabulka]</span><span class="sxs-lookup"><span data-stu-id="4bc2b-311">dbo1.MyTable or [dbo1].[MyTable]</span></span> |
| <span data-ttu-id="4bc2b-312">dbo</span><span class="sxs-lookup"><span data-stu-id="4bc2b-312">dbo</span></span> |<span data-ttu-id="4bc2b-313">My.Table</span><span class="sxs-lookup"><span data-stu-id="4bc2b-313">My.Table</span></span> |<span data-ttu-id="4bc2b-314">[My.Table] nebo [dbo]. [My.Table]</span><span class="sxs-lookup"><span data-stu-id="4bc2b-314">[My.Table] or [dbo].[My.Table]</span></span> |
| <span data-ttu-id="4bc2b-315">dbo1</span><span class="sxs-lookup"><span data-stu-id="4bc2b-315">dbo1</span></span> |<span data-ttu-id="4bc2b-316">My.Table</span><span class="sxs-lookup"><span data-stu-id="4bc2b-316">My.Table</span></span> |<span data-ttu-id="4bc2b-317">[dbo1]. [My.Table]</span><span class="sxs-lookup"><span data-stu-id="4bc2b-317">[dbo1].[My.Table]</span></span> |

<span data-ttu-id="4bc2b-318">Pokud se zobrazí následující chyba, může to být problém s hodnotou, kterou jste zadali pro vlastnost tableName.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-318">If you see the following error, it could be an issue with the value you specified for the tableName property.</span></span> <span data-ttu-id="4bc2b-319">Najdete v tabulce pro správný způsob, jak určit hodnoty pro vlastnost tableName JSON.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-319">See the table for the correct way to specify values for the tableName JSON property.</span></span>  

```
Type=System.Data.SqlClient.SqlException,Message=Invalid object name 'stg.Account_test'.,Source=.Net SqlClient Data Provider
```

### <a name="columns-with-default-values"></a><span data-ttu-id="4bc2b-320">Sloupce s výchozími hodnotami</span><span class="sxs-lookup"><span data-stu-id="4bc2b-320">Columns with default values</span></span>
<span data-ttu-id="4bc2b-321">V současné době funkce PolyBase v datové továrně přijímá pouze stejný počet sloupců jako v cílové tabulce.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-321">Currently, PolyBase feature in Data Factory only accepts the same number of columns as in the target table.</span></span> <span data-ttu-id="4bc2b-322">Řekněme, máte tabulku s čtyři sloupce a jeden z nich je definovaná pomocí výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-322">Say, you have a table with four columns and one of them is defined with a default value.</span></span> <span data-ttu-id="4bc2b-323">Vstupní data stále by mělo obsahovat čtyři sloupce.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-323">The input data should still contain four columns.</span></span> <span data-ttu-id="4bc2b-324">Poskytnutí 3sloupce vstupní datové sady, které by poskytují chyba podobná následující zpráva:</span><span class="sxs-lookup"><span data-stu-id="4bc2b-324">Providing a 3-column input dataset would yield an error similar to the following message:</span></span>

```
All columns of the table must be specified in the INSERT BULK statement.
```
<span data-ttu-id="4bc2b-325">Hodnota NULL je speciální forma výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-325">NULL value is a special form of default value.</span></span> <span data-ttu-id="4bc2b-326">Pokud je sloupec s možnou hodnotou Null, vstupní data (v objektu blob) pro tento sloupec může být prázdná (nelze chybí vstupní datové sady).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-326">If the column is nullable, the input data (in blob) for that column could be empty (cannot be missing from the input dataset).</span></span> <span data-ttu-id="4bc2b-327">PolyBase vloží NULL u nich v Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-327">PolyBase inserts NULL for them in the Azure SQL Data Warehouse.</span></span>  

## <a name="auto-table-creation"></a><span data-ttu-id="4bc2b-328">Automaticky vytvářené tabulky</span><span class="sxs-lookup"><span data-stu-id="4bc2b-328">Auto table creation</span></span>
<span data-ttu-id="4bc2b-329">Pokud použijete Průvodce kopírováním ke zkopírování dat z SQL serveru nebo databáze SQL Azure do Azure SQL Data Warehouse a tabulky, která odpovídá zdrojová tabulka neexistuje v cílové úložiště, Data Factory můžete automaticky vytvořit v tabulce v datovém skladu pomocí schématu zdrojové tabulky.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-329">If you are using Copy Wizard to copy data from SQL Server or Azure SQL Database to Azure SQL Data Warehouse and the table that corresponds to the source table does not exist in the destination store, Data Factory can automatically create the table in the data warehouse by using the source table schema.</span></span>

<span data-ttu-id="4bc2b-330">Data Factory vytvoří v cílové úložiště se stejným názvem tabulky v zdrojového úložiště dat v tabulce.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-330">Data Factory creates the table in the destination store with the same table name in the source data store.</span></span> <span data-ttu-id="4bc2b-331">Datové typy pro sloupce jsou zvolen v závislosti na následující mapování typu.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-331">The data types for columns are chosen based on the following type mapping.</span></span> <span data-ttu-id="4bc2b-332">V případě potřeby provede převody typů opravit všechny problémům s kompatibilitou mezi zdrojovým a cílovým úložišti.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-332">If needed, it performs type conversions to fix any incompatibilities between source and destination stores.</span></span> <span data-ttu-id="4bc2b-333">Používá také distribuce kruhové dotazování tabulky.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-333">It also uses Round Robin table distribution.</span></span>

| <span data-ttu-id="4bc2b-334">Typ sloupce zdrojové databáze SQL</span><span class="sxs-lookup"><span data-stu-id="4bc2b-334">Source SQL Database column type</span></span> | <span data-ttu-id="4bc2b-335">Cílový typ sloupce SQL DW (omezení velikosti)</span><span class="sxs-lookup"><span data-stu-id="4bc2b-335">Destination SQL DW column type (size limitation)</span></span> |
| --- | --- |
| <span data-ttu-id="4bc2b-336">celá čísla</span><span class="sxs-lookup"><span data-stu-id="4bc2b-336">Int</span></span> | <span data-ttu-id="4bc2b-337">celá čísla</span><span class="sxs-lookup"><span data-stu-id="4bc2b-337">Int</span></span> |
| <span data-ttu-id="4bc2b-338">BigInt</span><span class="sxs-lookup"><span data-stu-id="4bc2b-338">BigInt</span></span> | <span data-ttu-id="4bc2b-339">BigInt</span><span class="sxs-lookup"><span data-stu-id="4bc2b-339">BigInt</span></span> |
| <span data-ttu-id="4bc2b-340">SmallInt</span><span class="sxs-lookup"><span data-stu-id="4bc2b-340">SmallInt</span></span> | <span data-ttu-id="4bc2b-341">SmallInt</span><span class="sxs-lookup"><span data-stu-id="4bc2b-341">SmallInt</span></span> |
| <span data-ttu-id="4bc2b-342">TinyInt</span><span class="sxs-lookup"><span data-stu-id="4bc2b-342">TinyInt</span></span> | <span data-ttu-id="4bc2b-343">TinyInt</span><span class="sxs-lookup"><span data-stu-id="4bc2b-343">TinyInt</span></span> |
| <span data-ttu-id="4bc2b-344">Bit</span><span class="sxs-lookup"><span data-stu-id="4bc2b-344">Bit</span></span> | <span data-ttu-id="4bc2b-345">Bit</span><span class="sxs-lookup"><span data-stu-id="4bc2b-345">Bit</span></span> |
| <span data-ttu-id="4bc2b-346">Decimal</span><span class="sxs-lookup"><span data-stu-id="4bc2b-346">Decimal</span></span> | <span data-ttu-id="4bc2b-347">Decimal</span><span class="sxs-lookup"><span data-stu-id="4bc2b-347">Decimal</span></span> |
| <span data-ttu-id="4bc2b-348">číselné</span><span class="sxs-lookup"><span data-stu-id="4bc2b-348">Numeric</span></span> | <span data-ttu-id="4bc2b-349">Decimal</span><span class="sxs-lookup"><span data-stu-id="4bc2b-349">Decimal</span></span> |
| <span data-ttu-id="4bc2b-350">Plovoucí desetinná čárka</span><span class="sxs-lookup"><span data-stu-id="4bc2b-350">Float</span></span> | <span data-ttu-id="4bc2b-351">Plovoucí desetinná čárka</span><span class="sxs-lookup"><span data-stu-id="4bc2b-351">Float</span></span> |
| <span data-ttu-id="4bc2b-352">peníze</span><span class="sxs-lookup"><span data-stu-id="4bc2b-352">Money</span></span> | <span data-ttu-id="4bc2b-353">peníze</span><span class="sxs-lookup"><span data-stu-id="4bc2b-353">Money</span></span> |
| <span data-ttu-id="4bc2b-354">Real</span><span class="sxs-lookup"><span data-stu-id="4bc2b-354">Real</span></span> | <span data-ttu-id="4bc2b-355">Real</span><span class="sxs-lookup"><span data-stu-id="4bc2b-355">Real</span></span> |
| <span data-ttu-id="4bc2b-356">SmallMoney</span><span class="sxs-lookup"><span data-stu-id="4bc2b-356">SmallMoney</span></span> | <span data-ttu-id="4bc2b-357">SmallMoney</span><span class="sxs-lookup"><span data-stu-id="4bc2b-357">SmallMoney</span></span> |
| <span data-ttu-id="4bc2b-358">Binární</span><span class="sxs-lookup"><span data-stu-id="4bc2b-358">Binary</span></span> | <span data-ttu-id="4bc2b-359">Binární</span><span class="sxs-lookup"><span data-stu-id="4bc2b-359">Binary</span></span> |
| <span data-ttu-id="4bc2b-360">varbinary</span><span class="sxs-lookup"><span data-stu-id="4bc2b-360">Varbinary</span></span> | <span data-ttu-id="4bc2b-361">Varbinary (až 8000)</span><span class="sxs-lookup"><span data-stu-id="4bc2b-361">Varbinary (up to 8000)</span></span> |
| <span data-ttu-id="4bc2b-362">Datum</span><span class="sxs-lookup"><span data-stu-id="4bc2b-362">Date</span></span> | <span data-ttu-id="4bc2b-363">Datum</span><span class="sxs-lookup"><span data-stu-id="4bc2b-363">Date</span></span> |
| <span data-ttu-id="4bc2b-364">Data a času</span><span class="sxs-lookup"><span data-stu-id="4bc2b-364">DateTime</span></span> | <span data-ttu-id="4bc2b-365">Data a času</span><span class="sxs-lookup"><span data-stu-id="4bc2b-365">DateTime</span></span> |
| <span data-ttu-id="4bc2b-366">DateTime2</span><span class="sxs-lookup"><span data-stu-id="4bc2b-366">DateTime2</span></span> | <span data-ttu-id="4bc2b-367">DateTime2</span><span class="sxs-lookup"><span data-stu-id="4bc2b-367">DateTime2</span></span> |
| <span data-ttu-id="4bc2b-368">Čas</span><span class="sxs-lookup"><span data-stu-id="4bc2b-368">Time</span></span> | <span data-ttu-id="4bc2b-369">Čas</span><span class="sxs-lookup"><span data-stu-id="4bc2b-369">Time</span></span> |
| <span data-ttu-id="4bc2b-370">Datový typ DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="4bc2b-370">DateTimeOffset</span></span> | <span data-ttu-id="4bc2b-371">Datový typ DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="4bc2b-371">DateTimeOffset</span></span> |
| <span data-ttu-id="4bc2b-372">SmallDateTime</span><span class="sxs-lookup"><span data-stu-id="4bc2b-372">SmallDateTime</span></span> | <span data-ttu-id="4bc2b-373">SmallDateTime</span><span class="sxs-lookup"><span data-stu-id="4bc2b-373">SmallDateTime</span></span> |
| <span data-ttu-id="4bc2b-374">Text</span><span class="sxs-lookup"><span data-stu-id="4bc2b-374">Text</span></span> | <span data-ttu-id="4bc2b-375">Varchar (až 8000)</span><span class="sxs-lookup"><span data-stu-id="4bc2b-375">Varchar (up to 8000)</span></span> |
| <span data-ttu-id="4bc2b-376">NText</span><span class="sxs-lookup"><span data-stu-id="4bc2b-376">NText</span></span> | <span data-ttu-id="4bc2b-377">NVarChar (až 4000)</span><span class="sxs-lookup"><span data-stu-id="4bc2b-377">NVarChar (up to 4000)</span></span> |
| <span data-ttu-id="4bc2b-378">Image</span><span class="sxs-lookup"><span data-stu-id="4bc2b-378">Image</span></span> | <span data-ttu-id="4bc2b-379">VarBinary (až 8000)</span><span class="sxs-lookup"><span data-stu-id="4bc2b-379">VarBinary (up to 8000)</span></span> |
| <span data-ttu-id="4bc2b-380">Typ UniqueIdentifier</span><span class="sxs-lookup"><span data-stu-id="4bc2b-380">UniqueIdentifier</span></span> | <span data-ttu-id="4bc2b-381">Typ UniqueIdentifier</span><span class="sxs-lookup"><span data-stu-id="4bc2b-381">UniqueIdentifier</span></span> |
| <span data-ttu-id="4bc2b-382">Char</span><span class="sxs-lookup"><span data-stu-id="4bc2b-382">Char</span></span> | <span data-ttu-id="4bc2b-383">Char</span><span class="sxs-lookup"><span data-stu-id="4bc2b-383">Char</span></span> |
| <span data-ttu-id="4bc2b-384">NChar</span><span class="sxs-lookup"><span data-stu-id="4bc2b-384">NChar</span></span> | <span data-ttu-id="4bc2b-385">NChar</span><span class="sxs-lookup"><span data-stu-id="4bc2b-385">NChar</span></span> |
| <span data-ttu-id="4bc2b-386">VarChar</span><span class="sxs-lookup"><span data-stu-id="4bc2b-386">VarChar</span></span> | <span data-ttu-id="4bc2b-387">VarChar (až 8000)</span><span class="sxs-lookup"><span data-stu-id="4bc2b-387">VarChar (up to 8000)</span></span> |
| <span data-ttu-id="4bc2b-388">NVarChar</span><span class="sxs-lookup"><span data-stu-id="4bc2b-388">NVarChar</span></span> | <span data-ttu-id="4bc2b-389">NVarChar (až 4000)</span><span class="sxs-lookup"><span data-stu-id="4bc2b-389">NVarChar (up to 4000)</span></span> |
| <span data-ttu-id="4bc2b-390">XML</span><span class="sxs-lookup"><span data-stu-id="4bc2b-390">Xml</span></span> | <span data-ttu-id="4bc2b-391">Varchar (až 8000)</span><span class="sxs-lookup"><span data-stu-id="4bc2b-391">Varchar (up to 8000)</span></span> |

[!INCLUDE [data-factory-type-repeatability-for-sql-sources](../../includes/data-factory-type-repeatability-for-sql-sources.md)]

## <a name="type-mapping-for-azure-sql-data-warehouse"></a><span data-ttu-id="4bc2b-392">Mapování typu pro Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="4bc2b-392">Type mapping for Azure SQL Data Warehouse</span></span>
<span data-ttu-id="4bc2b-393">Jak je uvedeno v [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů zdroje do jímky typů s následující postup krok 2:</span><span class="sxs-lookup"><span data-stu-id="4bc2b-393">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types to sink types with the following 2-step approach:</span></span>

1. <span data-ttu-id="4bc2b-394">Převést na typ .NET typy nativní zdrojů</span><span class="sxs-lookup"><span data-stu-id="4bc2b-394">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="4bc2b-395">Převést na typ jímky nativní typ formátu .NET</span><span class="sxs-lookup"><span data-stu-id="4bc2b-395">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="4bc2b-396">Při přesunu dat do a z Azure SQL Data Warehouse, se používají následující mapování z typu SQL na typ .NET a naopak.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-396">When moving data to & from Azure SQL Data Warehouse, the following mappings are used from SQL type to .NET type and vice versa.</span></span>

<span data-ttu-id="4bc2b-397">Mapování je stejné jako [mapování datového typu aplikace SQL Server pro technologii ADO.NET](https://msdn.microsoft.com/library/cc716729.aspx).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-397">The mapping is same as the [SQL Server Data Type Mapping for ADO.NET](https://msdn.microsoft.com/library/cc716729.aspx).</span></span>

| <span data-ttu-id="4bc2b-398">Typ databázového stroje SQL Server</span><span class="sxs-lookup"><span data-stu-id="4bc2b-398">SQL Server Database Engine type</span></span> | <span data-ttu-id="4bc2b-399">Typ rozhraní .NET framework</span><span class="sxs-lookup"><span data-stu-id="4bc2b-399">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="4bc2b-400">bigint</span><span class="sxs-lookup"><span data-stu-id="4bc2b-400">bigint</span></span> |<span data-ttu-id="4bc2b-401">Int64</span><span class="sxs-lookup"><span data-stu-id="4bc2b-401">Int64</span></span> |
| <span data-ttu-id="4bc2b-402">Binární</span><span class="sxs-lookup"><span data-stu-id="4bc2b-402">binary</span></span> |<span data-ttu-id="4bc2b-403">Byte]</span><span class="sxs-lookup"><span data-stu-id="4bc2b-403">Byte[]</span></span> |
| <span data-ttu-id="4bc2b-404">Bit</span><span class="sxs-lookup"><span data-stu-id="4bc2b-404">bit</span></span> |<span data-ttu-id="4bc2b-405">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="4bc2b-405">Boolean</span></span> |
| <span data-ttu-id="4bc2b-406">Char</span><span class="sxs-lookup"><span data-stu-id="4bc2b-406">char</span></span> |<span data-ttu-id="4bc2b-407">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="4bc2b-407">String, Char[]</span></span> |
| <span data-ttu-id="4bc2b-408">Datum</span><span class="sxs-lookup"><span data-stu-id="4bc2b-408">date</span></span> |<span data-ttu-id="4bc2b-409">Data a času</span><span class="sxs-lookup"><span data-stu-id="4bc2b-409">DateTime</span></span> |
| <span data-ttu-id="4bc2b-410">Data a času</span><span class="sxs-lookup"><span data-stu-id="4bc2b-410">Datetime</span></span> |<span data-ttu-id="4bc2b-411">Data a času</span><span class="sxs-lookup"><span data-stu-id="4bc2b-411">DateTime</span></span> |
| <span data-ttu-id="4bc2b-412">datetime2</span><span class="sxs-lookup"><span data-stu-id="4bc2b-412">datetime2</span></span> |<span data-ttu-id="4bc2b-413">Data a času</span><span class="sxs-lookup"><span data-stu-id="4bc2b-413">DateTime</span></span> |
| <span data-ttu-id="4bc2b-414">Datový typ DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="4bc2b-414">Datetimeoffset</span></span> |<span data-ttu-id="4bc2b-415">Datový typ DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="4bc2b-415">DateTimeOffset</span></span> |
| <span data-ttu-id="4bc2b-416">Decimal</span><span class="sxs-lookup"><span data-stu-id="4bc2b-416">Decimal</span></span> |<span data-ttu-id="4bc2b-417">Decimal</span><span class="sxs-lookup"><span data-stu-id="4bc2b-417">Decimal</span></span> |
| <span data-ttu-id="4bc2b-418">Atribut FILESTREAM (varbinary(max))</span><span class="sxs-lookup"><span data-stu-id="4bc2b-418">FILESTREAM attribute (varbinary(max))</span></span> |<span data-ttu-id="4bc2b-419">Byte]</span><span class="sxs-lookup"><span data-stu-id="4bc2b-419">Byte[]</span></span> |
| <span data-ttu-id="4bc2b-420">Plovoucí desetinná čárka</span><span class="sxs-lookup"><span data-stu-id="4bc2b-420">Float</span></span> |<span data-ttu-id="4bc2b-421">Double</span><span class="sxs-lookup"><span data-stu-id="4bc2b-421">Double</span></span> |
| <span data-ttu-id="4bc2b-422">Bitové kopie</span><span class="sxs-lookup"><span data-stu-id="4bc2b-422">image</span></span> |<span data-ttu-id="4bc2b-423">Byte]</span><span class="sxs-lookup"><span data-stu-id="4bc2b-423">Byte[]</span></span> |
| <span data-ttu-id="4bc2b-424">celá čísla</span><span class="sxs-lookup"><span data-stu-id="4bc2b-424">int</span></span> |<span data-ttu-id="4bc2b-425">Int32</span><span class="sxs-lookup"><span data-stu-id="4bc2b-425">Int32</span></span> |
| <span data-ttu-id="4bc2b-426">peníze</span><span class="sxs-lookup"><span data-stu-id="4bc2b-426">money</span></span> |<span data-ttu-id="4bc2b-427">Decimal</span><span class="sxs-lookup"><span data-stu-id="4bc2b-427">Decimal</span></span> |
| <span data-ttu-id="4bc2b-428">nchar</span><span class="sxs-lookup"><span data-stu-id="4bc2b-428">nchar</span></span> |<span data-ttu-id="4bc2b-429">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="4bc2b-429">String, Char[]</span></span> |
| <span data-ttu-id="4bc2b-430">ntext</span><span class="sxs-lookup"><span data-stu-id="4bc2b-430">ntext</span></span> |<span data-ttu-id="4bc2b-431">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="4bc2b-431">String, Char[]</span></span> |
| <span data-ttu-id="4bc2b-432">číselné</span><span class="sxs-lookup"><span data-stu-id="4bc2b-432">numeric</span></span> |<span data-ttu-id="4bc2b-433">Decimal</span><span class="sxs-lookup"><span data-stu-id="4bc2b-433">Decimal</span></span> |
| <span data-ttu-id="4bc2b-434">nvarchar</span><span class="sxs-lookup"><span data-stu-id="4bc2b-434">nvarchar</span></span> |<span data-ttu-id="4bc2b-435">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="4bc2b-435">String, Char[]</span></span> |
| <span data-ttu-id="4bc2b-436">skutečné</span><span class="sxs-lookup"><span data-stu-id="4bc2b-436">real</span></span> |<span data-ttu-id="4bc2b-437">Jeden</span><span class="sxs-lookup"><span data-stu-id="4bc2b-437">Single</span></span> |
| <span data-ttu-id="4bc2b-438">ROWVERSION</span><span class="sxs-lookup"><span data-stu-id="4bc2b-438">rowversion</span></span> |<span data-ttu-id="4bc2b-439">Byte]</span><span class="sxs-lookup"><span data-stu-id="4bc2b-439">Byte[]</span></span> |
| <span data-ttu-id="4bc2b-440">smalldatetime</span><span class="sxs-lookup"><span data-stu-id="4bc2b-440">smalldatetime</span></span> |<span data-ttu-id="4bc2b-441">Data a času</span><span class="sxs-lookup"><span data-stu-id="4bc2b-441">DateTime</span></span> |
| <span data-ttu-id="4bc2b-442">smallint</span><span class="sxs-lookup"><span data-stu-id="4bc2b-442">smallint</span></span> |<span data-ttu-id="4bc2b-443">Int16</span><span class="sxs-lookup"><span data-stu-id="4bc2b-443">Int16</span></span> |
| <span data-ttu-id="4bc2b-444">Smallmoney</span><span class="sxs-lookup"><span data-stu-id="4bc2b-444">smallmoney</span></span> |<span data-ttu-id="4bc2b-445">Decimal</span><span class="sxs-lookup"><span data-stu-id="4bc2b-445">Decimal</span></span> |
| <span data-ttu-id="4bc2b-446">SQL_VARIANT</span><span class="sxs-lookup"><span data-stu-id="4bc2b-446">sql_variant</span></span> |<span data-ttu-id="4bc2b-447">Objekt *</span><span class="sxs-lookup"><span data-stu-id="4bc2b-447">Object *</span></span> |
| <span data-ttu-id="4bc2b-448">Text</span><span class="sxs-lookup"><span data-stu-id="4bc2b-448">text</span></span> |<span data-ttu-id="4bc2b-449">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="4bc2b-449">String, Char[]</span></span> |
| <span data-ttu-id="4bc2b-450">time</span><span class="sxs-lookup"><span data-stu-id="4bc2b-450">time</span></span> |<span data-ttu-id="4bc2b-451">Časový interval</span><span class="sxs-lookup"><span data-stu-id="4bc2b-451">TimeSpan</span></span> |
| <span data-ttu-id="4bc2b-452">časové razítko</span><span class="sxs-lookup"><span data-stu-id="4bc2b-452">timestamp</span></span> |<span data-ttu-id="4bc2b-453">Byte]</span><span class="sxs-lookup"><span data-stu-id="4bc2b-453">Byte[]</span></span> |
| <span data-ttu-id="4bc2b-454">tinyint</span><span class="sxs-lookup"><span data-stu-id="4bc2b-454">tinyint</span></span> |<span data-ttu-id="4bc2b-455">Bajtů</span><span class="sxs-lookup"><span data-stu-id="4bc2b-455">Byte</span></span> |
| <span data-ttu-id="4bc2b-456">Typ UniqueIdentifier</span><span class="sxs-lookup"><span data-stu-id="4bc2b-456">uniqueidentifier</span></span> |<span data-ttu-id="4bc2b-457">Identifikátor GUID</span><span class="sxs-lookup"><span data-stu-id="4bc2b-457">Guid</span></span> |
| <span data-ttu-id="4bc2b-458">varbinary</span><span class="sxs-lookup"><span data-stu-id="4bc2b-458">varbinary</span></span> |<span data-ttu-id="4bc2b-459">Byte]</span><span class="sxs-lookup"><span data-stu-id="4bc2b-459">Byte[]</span></span> |
| <span data-ttu-id="4bc2b-460">varchar</span><span class="sxs-lookup"><span data-stu-id="4bc2b-460">varchar</span></span> |<span data-ttu-id="4bc2b-461">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="4bc2b-461">String, Char[]</span></span> |
| <span data-ttu-id="4bc2b-462">xml</span><span class="sxs-lookup"><span data-stu-id="4bc2b-462">xml</span></span> |<span data-ttu-id="4bc2b-463">XML</span><span class="sxs-lookup"><span data-stu-id="4bc2b-463">Xml</span></span> |

<span data-ttu-id="4bc2b-464">Můžete také mapovat sloupců z datové sady zdroje na sloupce ze sady jímku dat v definici aktivity kopírování.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-464">You can also map columns from source dataset to columns from sink dataset in the copy activity definition.</span></span> <span data-ttu-id="4bc2b-465">Podrobnosti najdete v tématu [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-465">For details, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="json-examples-for-copying-data-to-and-from-sql-data-warehouse"></a><span data-ttu-id="4bc2b-466">Příklady JSON pro kopírování dat do a z SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="4bc2b-466">JSON examples for copying data to and from SQL Data Warehouse</span></span>
<span data-ttu-id="4bc2b-467">Následující příklady poskytují ukázka JSON definice, které můžete použít k vytvoření kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-467">The following examples provide sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="4bc2b-468">Se ukazují, jak ke zkopírování dat do a z Azure SQL Data Warehouse a Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-468">They show how to copy data to and from Azure SQL Data Warehouse and Azure Blob Storage.</span></span> <span data-ttu-id="4bc2b-469">Nicméně je možné zkopírovat data **přímo** ze všech zdrojů do jakéhokoli z jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí aktivity kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-469">However, data can be copied **directly** from any of sources to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>

### <a name="example-copy-data-from-azure-sql-data-warehouse-to-azure-blob"></a><span data-ttu-id="4bc2b-470">Příklad: Kopírování dat z Azure SQL Data Warehouse do objektů Blob v Azure</span><span class="sxs-lookup"><span data-stu-id="4bc2b-470">Example: Copy data from Azure SQL Data Warehouse to Azure Blob</span></span>
<span data-ttu-id="4bc2b-471">Ukázka definuje následující entity služby Data Factory:</span><span class="sxs-lookup"><span data-stu-id="4bc2b-471">The sample defines the following Data Factory entities:</span></span>

1. <span data-ttu-id="4bc2b-472">Propojené služby typu [AzureSqlDW](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-472">A linked service of type [AzureSqlDW](#linked-service-properties).</span></span>
2. <span data-ttu-id="4bc2b-473">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-473">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="4bc2b-474">Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureSqlDWTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-474">An input [dataset](data-factory-create-datasets.md) of type [AzureSqlDWTable](#dataset-properties).</span></span>
4. <span data-ttu-id="4bc2b-475">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-475">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="4bc2b-476">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [SqlDWSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-476">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [SqlDWSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="4bc2b-477">Ukázka zkopíruje data časové řady (hodinové, denní, atd.) z tabulky v databázi Azure SQL Data Warehouse do objektu blob každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-477">The sample copies time-series (hourly, daily, etc.) data from a table in Azure SQL Data Warehouse database to a blob every hour.</span></span> <span data-ttu-id="4bc2b-478">Vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-478">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="4bc2b-479">**Azure SQL Data Warehouse propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="4bc2b-479">**Azure SQL Data Warehouse linked service:**</span></span>

```JSON
{
  "name": "AzureSqlDWLinkedService",
  "properties": {
    "type": "AzureSqlDW",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
<span data-ttu-id="4bc2b-480">**Propojená služba Azure Blob storage:**</span><span class="sxs-lookup"><span data-stu-id="4bc2b-480">**Azure Blob storage linked service:**</span></span>

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
<span data-ttu-id="4bc2b-481">**Azure SQL Data Warehouse vstupní datové sady:**</span><span class="sxs-lookup"><span data-stu-id="4bc2b-481">**Azure SQL Data Warehouse input dataset:**</span></span>

<span data-ttu-id="4bc2b-482">Příkladu se předpokládá, jste vytvořili tabulku "MyTable" v Azure SQL Data Warehouse a obsahuje sloupec s názvem "timestampcolumn" pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-482">The sample assumes you have created a table “MyTable” in Azure SQL Data Warehouse and it contains a column called “timestampcolumn” for time series data.</span></span>

<span data-ttu-id="4bc2b-483">Nastavení "externí": "PRAVDA" informuje služba Data Factory, datová sada je externí k objektu pro vytváření dat a není vyprodukované aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-483">Setting “external”: ”true” informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

```JSON
{
  "name": "AzureSqlDWInput",
  "properties": {
    "type": "AzureSqlDWTable",
    "linkedServiceName": "AzureSqlDWLinkedService",
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
<span data-ttu-id="4bc2b-484">**Azure Blob výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="4bc2b-484">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="4bc2b-485">Data se zapisují do nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-485">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="4bc2b-486">Cesta ke složce pro tento objekt blob je vyhodnocován dynamicky podle času zahájení řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-486">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="4bc2b-487">Cesta ke složce používá rok, měsíc, den a čas částí čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-487">The folder path uses year, month, day, and hours parts of the start time.</span></span>

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

<span data-ttu-id="4bc2b-488">**Aktivita kopírování v kanálu s SqlDWSource a BlobSink:**</span><span class="sxs-lookup"><span data-stu-id="4bc2b-488">**Copy activity in a pipeline with SqlDWSource and BlobSink:**</span></span>

<span data-ttu-id="4bc2b-489">Kanál obsahuje aktivitu kopírování, který je nakonfigurovaný na použití vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-489">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="4bc2b-490">V definici JSON kanálu **zdroj** je typ nastaven na **SqlDWSource** a **podřízený** je typ nastaven na **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-490">In the pipeline JSON definition, the **source** type is set to **SqlDWSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="4bc2b-491">Zadané pro dotaz SQL **SqlReaderQuery** vlastnost vybere data za poslední hodinu pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-491">The SQL query specified for the **SqlReaderQuery** property selects the data in the past hour to copy.</span></span>

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "AzureSQLDWtoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureSqlDWInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlDWSource",
            "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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
> <span data-ttu-id="4bc2b-492">V příkladu **sqlReaderQuery** je zadán pro SqlDWSource.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-492">In the example, **sqlReaderQuery** is specified for the SqlDWSource.</span></span> <span data-ttu-id="4bc2b-493">Aktivita kopírování spustí tohoto dotazu na zdroji Azure SQL Data Warehouse se získat data.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-493">The Copy Activity runs this query against the Azure SQL Data Warehouse source to get the data.</span></span>
>
> <span data-ttu-id="4bc2b-494">Alternativně můžete zadat uložené procedury zadáním **sqlReaderStoredProcedureName** a **storedProcedureParameters** (Pokud uložená procedura přebírá parametry).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-494">Alternatively, you can specify a stored procedure by specifying the **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if the stored procedure takes parameters).</span></span>
>
> <span data-ttu-id="4bc2b-495">Pokud nezadáte sqlReaderQuery nebo sqlReaderStoredProcedureName, sloupce definované v části Struktura sady dat JSON se používají k vytvoření dotazu (vyberte Sloupec1, Sloupec2 z mytable) ke spouštění na Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-495">If you do not specify either sqlReaderQuery or sqlReaderStoredProcedureName, the columns defined in the structure section of the dataset JSON are used to build a query (select column1, column2 from mytable) to run against the Azure SQL Data Warehouse.</span></span> <span data-ttu-id="4bc2b-496">Pokud definice datové sady nemá strukturu, jsou vybrány všechny sloupce z tabulky.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-496">If the dataset definition does not have the structure, all columns are selected from the table.</span></span>
>
>

### <a name="example-copy-data-from-azure-blob-to-azure-sql-data-warehouse"></a><span data-ttu-id="4bc2b-497">Příklad: Kopírování dat z objektu Blob Azure do Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="4bc2b-497">Example: Copy data from Azure Blob to Azure SQL Data Warehouse</span></span>
<span data-ttu-id="4bc2b-498">Ukázka definuje následující entity služby Data Factory:</span><span class="sxs-lookup"><span data-stu-id="4bc2b-498">The sample defines the following Data Factory entities:</span></span>

1. <span data-ttu-id="4bc2b-499">Propojené služby typu [AzureSqlDW](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-499">A linked service of type [AzureSqlDW](#linked-service-properties).</span></span>
2. <span data-ttu-id="4bc2b-500">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-500">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="4bc2b-501">Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-501">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="4bc2b-502">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureSqlDWTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-502">An output [dataset](data-factory-create-datasets.md) of type [AzureSqlDWTable](#dataset-properties).</span></span>
5. <span data-ttu-id="4bc2b-503">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) a [SqlDWSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-503">A [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) and [SqlDWSink](#copy-activity-properties).</span></span>

<span data-ttu-id="4bc2b-504">Kopie ukázka časové řady dat (hodinový, denní atd.) z Azure blob do tabulky v Azure SQL Data Warehouse databáze každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-504">The sample copies time-series data (hourly, daily, etc.) from Azure blob to a table in Azure SQL Data Warehouse database every hour.</span></span> <span data-ttu-id="4bc2b-505">Vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-505">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="4bc2b-506">**Azure SQL Data Warehouse propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="4bc2b-506">**Azure SQL Data Warehouse linked service:**</span></span>

```JSON
{
  "name": "AzureSqlDWLinkedService",
  "properties": {
    "type": "AzureSqlDW",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
<span data-ttu-id="4bc2b-507">**Propojená služba Azure Blob storage:**</span><span class="sxs-lookup"><span data-stu-id="4bc2b-507">**Azure Blob storage linked service:**</span></span>

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
<span data-ttu-id="4bc2b-508">**Azure vstupní datovou sadu objektu Blob:**</span><span class="sxs-lookup"><span data-stu-id="4bc2b-508">**Azure Blob input dataset:**</span></span>

<span data-ttu-id="4bc2b-509">Data je převzata z nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="4bc2b-509">Data is picked up from a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="4bc2b-510">Název složky a cesta k souboru pro tento objekt blob se vyhodnocují dynamicky podle času zahájení řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-510">The folder path and file name for the blob are dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="4bc2b-511">Cesta ke složce používá rok, měsíc a den součástí čas spuštění a název souboru používá hodinu součástí čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-511">The folder path uses year, month, and day part of the start time and file name uses the hour part of the start time.</span></span> <span data-ttu-id="4bc2b-512">"externí": "PRAVDA" nastavení informuje služba Data Factory, že tato tabulka je externí k objektu pro vytváření dat a není vyprodukované aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-512">“external”: “true” setting informs the Data Factory service that this table is external to the data factory and is not produced by an activity in the data factory.</span></span>

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
<span data-ttu-id="4bc2b-513">**Azure SQL Data Warehouse výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="4bc2b-513">**Azure SQL Data Warehouse output dataset:**</span></span>

<span data-ttu-id="4bc2b-514">Ukázka zkopíruje data na tabulku s názvem "MyTable" v Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-514">The sample copies data to a table named “MyTable” in Azure SQL Data Warehouse.</span></span> <span data-ttu-id="4bc2b-515">Vytvořte v tabulce v Azure SQL Data Warehouse s stejný počet sloupců, podle očekávání souboru CSV objektů Blob tak, aby obsahovala.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-515">Create the table in Azure SQL Data Warehouse with the same number of columns as you expect the Blob CSV file to contain.</span></span> <span data-ttu-id="4bc2b-516">Nové záznamy se přidají do tabulky každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-516">New rows are added to the table every hour.</span></span>

```JSON
{
  "name": "AzureSqlDWOutput",
  "properties": {
    "type": "AzureSqlDWTable",
    "linkedServiceName": "AzureSqlDWLinkedService",
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
<span data-ttu-id="4bc2b-517">**Aktivita kopírování v kanálu s BlobSource a SqlDWSink:**</span><span class="sxs-lookup"><span data-stu-id="4bc2b-517">**Copy activity in a pipeline with BlobSource and SqlDWSink:**</span></span>

<span data-ttu-id="4bc2b-518">Kanál obsahuje aktivitu kopírování, který je nakonfigurovaný na použití vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-518">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="4bc2b-519">V definici JSON kanálu **zdroj** je typ nastaven na **BlobSource** a **podřízený** je typ nastaven na **SqlDWSink**.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-519">In the pipeline JSON definition, the **source** type is set to **BlobSource** and **sink** type is set to **SqlDWSink**.</span></span>

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoSQLDW",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlDWOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource",
            "blobColumnSeparators": ","
          },
          "sink": {
            "type": "SqlDWSink",
            "allowPolyBase": true
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
<span data-ttu-id="4bc2b-520">Podrobný postup najdete [načíst 1 TB do Azure SQL Data Warehouse pomocí Azure Data Factory v části 15 minut](data-factory-load-sql-data-warehouse.md) a [načtení dat pomocí Azure Data Factory](../sql-data-warehouse/sql-data-warehouse-get-started-load-with-azure-data-factory.md) článek v dokumentaci k Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-520">For a walkthrough, see the see [Load 1 TB into Azure SQL Data Warehouse under 15 minutes with Azure Data Factory](data-factory-load-sql-data-warehouse.md) and [Load data with Azure Data Factory](../sql-data-warehouse/sql-data-warehouse-get-started-load-with-azure-data-factory.md) article in the Azure SQL Data Warehouse documentation.</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="4bc2b-521">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="4bc2b-521">Performance and Tuning</span></span>
<span data-ttu-id="4bc2b-522">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) Další informace o klíčových faktorů, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby, jak optimalizovat ho.</span><span class="sxs-lookup"><span data-stu-id="4bc2b-522">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
