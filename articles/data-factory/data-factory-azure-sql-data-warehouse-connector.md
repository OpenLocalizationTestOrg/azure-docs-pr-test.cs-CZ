---
title: aaaCopy data do/z Azure SQL Data Warehouse | Microsoft Docs
description: "Zjistěte, jak toocopy data do/z Azure SQL Data Warehouse pomocí Azure Data Factory"
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
ms.openlocfilehash: 75bfcf3c99844fc1297ca500107da23cf875e41f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tooand-from-azure-sql-data-warehouse-using-azure-data-factory"></a><span data-ttu-id="75f40-103">Tooand kopírování dat z Azure SQL Data Warehouse pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="75f40-103">Copy data tooand from Azure SQL Data Warehouse using Azure Data Factory</span></span>
<span data-ttu-id="75f40-104">Tento článek vysvětluje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove dat z Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="75f40-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data to/from Azure SQL Data Warehouse.</span></span> <span data-ttu-id="75f40-105">Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="75f40-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>  

> [!TIP]
> <span data-ttu-id="75f40-106">tooachieve co nejlepší výkon, použijte PolyBase tooload data do Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="75f40-106">tooachieve best performance, use PolyBase tooload data into Azure SQL Data Warehouse.</span></span> <span data-ttu-id="75f40-107">Hello [použijte PolyBase tooload data do Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) obsahuje podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="75f40-107">hello [Use PolyBase tooload data into Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) section has details.</span></span> <span data-ttu-id="75f40-108">Návod s případu použití najdete v tématu [načíst 1 TB do Azure SQL Data Warehouse pomocí Azure Data Factory v části 15 minut](data-factory-load-sql-data-warehouse.md).</span><span class="sxs-lookup"><span data-stu-id="75f40-108">For a walkthrough with a use case, see [Load 1 TB into Azure SQL Data Warehouse under 15 minutes with Azure Data Factory](data-factory-load-sql-data-warehouse.md).</span></span>

## <a name="supported-scenarios"></a><span data-ttu-id="75f40-109">Podporované scénáře</span><span class="sxs-lookup"><span data-stu-id="75f40-109">Supported scenarios</span></span>
<span data-ttu-id="75f40-110">Může kopírovat data **z Azure SQL Data Warehouse** toohello následující úložišť dat:</span><span class="sxs-lookup"><span data-stu-id="75f40-110">You can copy data **from Azure SQL Data Warehouse** toohello following data stores:</span></span>

[!INCLUDE [data-factory-supported-sinks](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="75f40-111">Data můžete zkopírovat z hello následující úložišť dat **tooAzure SQL Data Warehouse**:</span><span class="sxs-lookup"><span data-stu-id="75f40-111">You can copy data from hello following data stores **tooAzure SQL Data Warehouse**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

> [!TIP]
> <span data-ttu-id="75f40-112">Při kopírování dat z SQL serveru nebo Azure SQL Database tooAzure SQL Data Warehouse, pokud tabulka hello neexistuje v hello cílové úložiště, Data Factory můžete automaticky vytvořit hello tabulky v SQL Data Warehouse pomocí schématu hello hello tabulky ve zdroji hello úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="75f40-112">When copying data from SQL Server or Azure SQL Database tooAzure SQL Data Warehouse, if hello table does not exist in hello destination store, Data Factory can automatically create hello table in SQL Data Warehouse by using hello schema of hello table in hello source data store.</span></span> <span data-ttu-id="75f40-113">V tématu [automatické vytvoření tabulky](#auto-table-creation) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="75f40-113">See [Auto table creation](#auto-table-creation) for details.</span></span>

## <a name="supported-authentication-type"></a><span data-ttu-id="75f40-114">Podporované typ ověřování</span><span class="sxs-lookup"><span data-stu-id="75f40-114">Supported authentication type</span></span>
<span data-ttu-id="75f40-115">Azure SQL Data Warehouse konektor podporu základní ověřování.</span><span class="sxs-lookup"><span data-stu-id="75f40-115">Azure SQL Data Warehouse connector support basic authentication.</span></span>

## <a name="getting-started"></a><span data-ttu-id="75f40-116">Začínáme</span><span class="sxs-lookup"><span data-stu-id="75f40-116">Getting started</span></span>
<span data-ttu-id="75f40-117">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z Azure SQL Data Warehouse pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="75f40-117">You can create a pipeline with a copy activity that moves data to/from an Azure SQL Data Warehouse by using different tools/APIs.</span></span>

<span data-ttu-id="75f40-118">Nejjednodušší způsob, jak toocreate Hello kanál, který kopíruje data z Azure SQL Data Warehouse je toouse hello kopírování dat průvodce.</span><span class="sxs-lookup"><span data-stu-id="75f40-118">hello easiest way toocreate a pipeline that copies data to/from Azure SQL Data Warehouse is toouse hello Copy data wizard.</span></span> <span data-ttu-id="75f40-119">V tématu [kurz: načtení dat do SQL Data Warehouse pomocí služby Data Factory](../sql-data-warehouse/sql-data-warehouse-load-with-data-factory.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.</span><span class="sxs-lookup"><span data-stu-id="75f40-119">See [Tutorial: Load data into SQL Data Warehouse with Data Factory](../sql-data-warehouse/sql-data-warehouse-load-with-data-factory.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="75f40-120">Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**.</span><span class="sxs-lookup"><span data-stu-id="75f40-120">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="75f40-121">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="75f40-121">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span>

<span data-ttu-id="75f40-122">Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:</span><span class="sxs-lookup"><span data-stu-id="75f40-122">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="75f40-123">Vytvoření **objekt pro vytváření dat**.</span><span class="sxs-lookup"><span data-stu-id="75f40-123">Create a **data factory**.</span></span> <span data-ttu-id="75f40-124">Objekt pro vytváření dat může obsahovat jeden nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="75f40-124">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="75f40-125">Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="75f40-125">Create **linked services** toolink input and output data stores tooyour data factory.</span></span> <span data-ttu-id="75f40-126">Například pokud data kopírujete z Azure blob storage tooan Azure SQL data warehouse, vytvoříte dvě propojené služby toolink váš účet úložiště Azure a Azure SQL data warehouse tooyour objekt pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="75f40-126">For example, if you are copying data from an Azure blob storage tooan Azure SQL data warehouse, you create two linked services toolink your Azure storage account and Azure SQL data warehouse tooyour data factory.</span></span> <span data-ttu-id="75f40-127">Vlastnosti propojené služby, které jsou specifické tooAzure SQL Data Warehouse, najdete v části [propojené vlastnosti služby](#linked-service-properties) části.</span><span class="sxs-lookup"><span data-stu-id="75f40-127">For linked service properties that are specific tooAzure SQL Data Warehouse, see [linked service properties](#linked-service-properties) section.</span></span> 
3. <span data-ttu-id="75f40-128">Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="75f40-128">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> <span data-ttu-id="75f40-129">V příkladu hello uvedených v posledním kroku hello vytvořte kontejner objektů blob hello toospecify datovou sadu a složky, která obsahuje vstupní data hello.</span><span class="sxs-lookup"><span data-stu-id="75f40-129">In hello example mentioned in hello last step, you create a dataset toospecify hello blob container and folder that contains hello input data.</span></span> <span data-ttu-id="75f40-130">A vytvořte jinou tabulkou hello toospecify datovou sadu v hello Azure SQL data warehouse, která obsahuje hello daty zkopírovanými z úložiště objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="75f40-130">And, you create another dataset toospecify hello table in hello Azure SQL data warehouse that holds hello data copied from hello blob storage.</span></span> <span data-ttu-id="75f40-131">Vlastnosti datové sady, které jsou specifické tooAzure SQL Data Warehouse, najdete v části [vlastnosti datové sady](#dataset-properties) části.</span><span class="sxs-lookup"><span data-stu-id="75f40-131">For dataset properties that are specific tooAzure SQL Data Warehouse, see [dataset properties](#dataset-properties) section.</span></span>
4. <span data-ttu-id="75f40-132">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="75f40-132">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="75f40-133">V příkladu hello již bylo zmíněno dříve použijete BlobSource jako zdroj a SqlDWSink jako jímku pro aktivitu kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="75f40-133">In hello example mentioned earlier, you use BlobSource as a source and SqlDWSink as a sink for hello copy activity.</span></span> <span data-ttu-id="75f40-134">Podobně pokud zkopírujete z Azure SQL Data Warehouse tooAzure úložiště objektů Blob, použijte SqlDWSource a BlobSink v aktivitě kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="75f40-134">Similarly, if you are copying from Azure SQL Data Warehouse tooAzure Blob Storage, you use SqlDWSource and BlobSink in hello copy activity.</span></span> <span data-ttu-id="75f40-135">Vlastnosti aktivity kopírování, které jsou specifické tooAzure SQL Data Warehouse, najdete v části [zkopírovat vlastnosti aktivity](#copy-activity-properties) části.</span><span class="sxs-lookup"><span data-stu-id="75f40-135">For copy activity properties that are specific tooAzure SQL Data Warehouse, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="75f40-136">Podrobnosti o způsobu toouse úložiště dat jako zdroj nebo jímka klikněte na tlačítko hello odkaz v předchozí části hello pro data store.</span><span class="sxs-lookup"><span data-stu-id="75f40-136">For details on how toouse a data store as a source or a sink, click hello link in hello previous section for your data store.</span></span>

<span data-ttu-id="75f40-137">Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello).</span><span class="sxs-lookup"><span data-stu-id="75f40-137">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="75f40-138">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.</span><span class="sxs-lookup"><span data-stu-id="75f40-138">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="75f40-139">Ukázky s definicemi JSON entit služby Data Factory, které jsou používané toocopy data do/z Azure SQL Data Warehouse, najdete v části [JSON příklady](#json-examples-for-copying-data-to-and-from-sql-data-warehouse) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="75f40-139">For samples with JSON definitions for Data Factory entities that are used toocopy data to/from an Azure SQL Data Warehouse, see [JSON examples](#json-examples-for-copying-data-to-and-from-sql-data-warehouse) section of this article.</span></span>

<span data-ttu-id="75f40-140">Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooAzure SQL Data Warehouse:</span><span class="sxs-lookup"><span data-stu-id="75f40-140">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooAzure SQL Data Warehouse:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="75f40-141">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="75f40-141">Linked service properties</span></span>
<span data-ttu-id="75f40-142">Hello následující tabulka obsahuje popis pro konkrétní tooAzure elementy JSON SQL Data Warehouse propojené služby.</span><span class="sxs-lookup"><span data-stu-id="75f40-142">hello following table provides description for JSON elements specific tooAzure SQL Data Warehouse linked service.</span></span>

| <span data-ttu-id="75f40-143">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="75f40-143">Property</span></span> | <span data-ttu-id="75f40-144">Popis</span><span class="sxs-lookup"><span data-stu-id="75f40-144">Description</span></span> | <span data-ttu-id="75f40-145">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="75f40-145">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="75f40-146">type</span><span class="sxs-lookup"><span data-stu-id="75f40-146">type</span></span> |<span data-ttu-id="75f40-147">vlastnost typu Hello musí být nastavena na: **AzureSqlDW**</span><span class="sxs-lookup"><span data-stu-id="75f40-147">hello type property must be set to: **AzureSqlDW**</span></span> |<span data-ttu-id="75f40-148">Ano</span><span class="sxs-lookup"><span data-stu-id="75f40-148">Yes</span></span> |
| <span data-ttu-id="75f40-149">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="75f40-149">connectionString</span></span> |<span data-ttu-id="75f40-150">Zadejte informace potřebné pro vlastnost connectionString hello tooconnect toohello Azure SQL Data Warehouse instance.</span><span class="sxs-lookup"><span data-stu-id="75f40-150">Specify information needed tooconnect toohello Azure SQL Data Warehouse instance for hello connectionString property.</span></span> <span data-ttu-id="75f40-151">Podporováno je pouze základní ověřování.</span><span class="sxs-lookup"><span data-stu-id="75f40-151">Only basic authentication is supported.</span></span> |<span data-ttu-id="75f40-152">Ano</span><span class="sxs-lookup"><span data-stu-id="75f40-152">Yes</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="75f40-153">Konfigurace [Firewall databáze Azure SQL](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) a databázový server hello příliš[povolit serveru hello tooaccess Azure Services](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure).</span><span class="sxs-lookup"><span data-stu-id="75f40-153">Configure [Azure SQL Database Firewall](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) and hello database server too[allow Azure Services tooaccess hello server](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure).</span></span> <span data-ttu-id="75f40-154">Kromě toho pokud kopírujete tooAzure dat SQL Data Warehouse z mimo Azure včetně z místního zdroje dat se brána objekt pro vytváření dat, nakonfigurujte příslušné rozsah IP adres pro hello počítač, který odesílá data tooAzure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="75f40-154">Additionally, if you are copying data tooAzure SQL Data Warehouse from outside Azure including from on-premises data sources with data factory gateway, configure appropriate IP address range for hello machine that is sending data tooAzure SQL Data Warehouse.</span></span>

## <a name="dataset-properties"></a><span data-ttu-id="75f40-155">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="75f40-155">Dataset properties</span></span>
<span data-ttu-id="75f40-156">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="75f40-156">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="75f40-157">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="75f40-157">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="75f40-158">část rámci typeProperties Hello se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello.</span><span class="sxs-lookup"><span data-stu-id="75f40-158">hello typeProperties section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="75f40-159">Hello **rámci typeProperties** části pro datovou sadu hello typu **AzureSqlDWTable** má hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="75f40-159">hello **typeProperties** section for hello dataset of type **AzureSqlDWTable** has hello following properties:</span></span>

| <span data-ttu-id="75f40-160">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="75f40-160">Property</span></span> | <span data-ttu-id="75f40-161">Popis</span><span class="sxs-lookup"><span data-stu-id="75f40-161">Description</span></span> | <span data-ttu-id="75f40-162">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="75f40-162">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="75f40-163">tableName</span><span class="sxs-lookup"><span data-stu-id="75f40-163">tableName</span></span> |<span data-ttu-id="75f40-164">Název hello tabulku nebo zobrazení v databázi Azure SQL Data Warehouse hello, která hello propojená služba odkazuje.</span><span class="sxs-lookup"><span data-stu-id="75f40-164">Name of hello table or view in hello Azure SQL Data Warehouse database that hello linked service refers to.</span></span> |<span data-ttu-id="75f40-165">Ano</span><span class="sxs-lookup"><span data-stu-id="75f40-165">Yes</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="75f40-166">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="75f40-166">Copy activity properties</span></span>
<span data-ttu-id="75f40-167">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="75f40-167">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="75f40-168">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="75f40-168">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

> [!NOTE]
> <span data-ttu-id="75f40-169">Hello aktivity kopírování přijímá pouze jeden vstup a vytváří jenom jeden výstup.</span><span class="sxs-lookup"><span data-stu-id="75f40-169">hello Copy Activity takes only one input and produces only one output.</span></span>

<span data-ttu-id="75f40-170">Vzhledem k tomu, vlastnosti dostupné v rámci typeProperties části hello hello aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="75f40-170">Whereas, properties available in hello typeProperties section of hello activity vary with each activity type.</span></span> <span data-ttu-id="75f40-171">Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="75f40-171">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

### <a name="sqldwsource"></a><span data-ttu-id="75f40-172">SqlDWSource</span><span class="sxs-lookup"><span data-stu-id="75f40-172">SqlDWSource</span></span>
<span data-ttu-id="75f40-173">Pokud je zdroj typu **SqlDWSource**, hello následující vlastnosti jsou k dispozici v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="75f40-173">When source is of type **SqlDWSource**, hello following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="75f40-174">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="75f40-174">Property</span></span> | <span data-ttu-id="75f40-175">Popis</span><span class="sxs-lookup"><span data-stu-id="75f40-175">Description</span></span> | <span data-ttu-id="75f40-176">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="75f40-176">Allowed values</span></span> | <span data-ttu-id="75f40-177">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="75f40-177">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="75f40-178">sqlReaderQuery</span><span class="sxs-lookup"><span data-stu-id="75f40-178">sqlReaderQuery</span></span> |<span data-ttu-id="75f40-179">Použijte data tooread hello vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="75f40-179">Use hello custom query tooread data.</span></span> |<span data-ttu-id="75f40-180">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="75f40-180">SQL query string.</span></span> <span data-ttu-id="75f40-181">Příklad: vybrat * z MyTable.</span><span class="sxs-lookup"><span data-stu-id="75f40-181">For example: select * from MyTable.</span></span> |<span data-ttu-id="75f40-182">Ne</span><span class="sxs-lookup"><span data-stu-id="75f40-182">No</span></span> |
| <span data-ttu-id="75f40-183">sqlReaderStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="75f40-183">sqlReaderStoredProcedureName</span></span> |<span data-ttu-id="75f40-184">Název hello uložené procedury, která čte data z hello zdrojové tabulky.</span><span class="sxs-lookup"><span data-stu-id="75f40-184">Name of hello stored procedure that reads data from hello source table.</span></span> |<span data-ttu-id="75f40-185">Název hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="75f40-185">Name of hello stored procedure.</span></span> <span data-ttu-id="75f40-186">Hello poslední příkaz jazyka SQL musí být příkaz SELECT v hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="75f40-186">hello last SQL statement must be a SELECT statement in hello stored procedure.</span></span> |<span data-ttu-id="75f40-187">Ne</span><span class="sxs-lookup"><span data-stu-id="75f40-187">No</span></span> |
| <span data-ttu-id="75f40-188">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="75f40-188">storedProcedureParameters</span></span> |<span data-ttu-id="75f40-189">Parametry pro hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="75f40-189">Parameters for hello stored procedure.</span></span> |<span data-ttu-id="75f40-190">Páry název/hodnota.</span><span class="sxs-lookup"><span data-stu-id="75f40-190">Name/value pairs.</span></span> <span data-ttu-id="75f40-191">Názvy a malá a velká písmena parametry musí odpovídat názvům hello a malá a velká písmena parametry hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="75f40-191">Names and casing of parameters must match hello names and casing of hello stored procedure parameters.</span></span> |<span data-ttu-id="75f40-192">Ne</span><span class="sxs-lookup"><span data-stu-id="75f40-192">No</span></span> |

<span data-ttu-id="75f40-193">Pokud hello **sqlReaderQuery** je zadán pro hello SqlDWSource, hello aktivity kopírování spouští tento dotaz hello Azure SQL Data Warehouse zdrojová tooget hello data.</span><span class="sxs-lookup"><span data-stu-id="75f40-193">If hello **sqlReaderQuery** is specified for hello SqlDWSource, hello Copy Activity runs this query against hello Azure SQL Data Warehouse source tooget hello data.</span></span>

<span data-ttu-id="75f40-194">Alternativně můžete zadat uložené procedury zadáním hello **sqlReaderStoredProcedureName** a **storedProcedureParameters** (Pokud hello uložená procedura používá parametry).</span><span class="sxs-lookup"><span data-stu-id="75f40-194">Alternatively, you can specify a stored procedure by specifying hello **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if hello stored procedure takes parameters).</span></span>

<span data-ttu-id="75f40-195">Pokud nezadáte sqlReaderQuery nebo sqlReaderStoredProcedureName, hello sloupce definovaný v oddílu Struktura hello hello sady dat JSON jsou použité toobuild toorun dotaz proti hello Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="75f40-195">If you do not specify either sqlReaderQuery or sqlReaderStoredProcedureName, hello columns defined in hello structure section of hello dataset JSON are used toobuild a query toorun against hello Azure SQL Data Warehouse.</span></span> <span data-ttu-id="75f40-196">Příklad: `select column1, column2 from mytable`.</span><span class="sxs-lookup"><span data-stu-id="75f40-196">Example: `select column1, column2 from mytable`.</span></span> <span data-ttu-id="75f40-197">Pokud definice datové sady hello nemá hello struktura, vyberou se všechny sloupce z tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="75f40-197">If hello dataset definition does not have hello structure, all columns are selected from hello table.</span></span>

#### <a name="sqldwsource-example"></a><span data-ttu-id="75f40-198">Příklad SqlDWSource</span><span class="sxs-lookup"><span data-stu-id="75f40-198">SqlDWSource example</span></span>

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
<span data-ttu-id="75f40-199">**definice Hello uložené procedury:**</span><span class="sxs-lookup"><span data-stu-id="75f40-199">**hello stored procedure definition:**</span></span>

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

### <a name="sqldwsink"></a><span data-ttu-id="75f40-200">SqlDWSink</span><span class="sxs-lookup"><span data-stu-id="75f40-200">SqlDWSink</span></span>
<span data-ttu-id="75f40-201">**SqlDWSink** podporuje hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="75f40-201">**SqlDWSink** supports hello following properties:</span></span>

| <span data-ttu-id="75f40-202">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="75f40-202">Property</span></span> | <span data-ttu-id="75f40-203">Popis</span><span class="sxs-lookup"><span data-stu-id="75f40-203">Description</span></span> | <span data-ttu-id="75f40-204">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="75f40-204">Allowed values</span></span> | <span data-ttu-id="75f40-205">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="75f40-205">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="75f40-206">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="75f40-206">sqlWriterCleanupScript</span></span> |<span data-ttu-id="75f40-207">Zadejte dotaz aktivity kopírování tooexecute tak, aby se vyčistit data určitý řez.</span><span class="sxs-lookup"><span data-stu-id="75f40-207">Specify a query for Copy Activity tooexecute such that data of a specific slice is cleaned up.</span></span> <span data-ttu-id="75f40-208">Podrobnosti najdete v tématu [opakovatelnosti části](#repeatability-during-copy).</span><span class="sxs-lookup"><span data-stu-id="75f40-208">For details, see [repeatability section](#repeatability-during-copy).</span></span> |<span data-ttu-id="75f40-209">Příkaz dotazu.</span><span class="sxs-lookup"><span data-stu-id="75f40-209">A query statement.</span></span> |<span data-ttu-id="75f40-210">Ne</span><span class="sxs-lookup"><span data-stu-id="75f40-210">No</span></span> |
| <span data-ttu-id="75f40-211">allowPolyBase</span><span class="sxs-lookup"><span data-stu-id="75f40-211">allowPolyBase</span></span> |<span data-ttu-id="75f40-212">Určuje, zda toouse PolyBase (v případě potřeby) namísto mechanismus hromadné vložení.</span><span class="sxs-lookup"><span data-stu-id="75f40-212">Indicates whether toouse PolyBase (when applicable) instead of BULKINSERT mechanism.</span></span> <br/><br/> <span data-ttu-id="75f40-213">**Hello doporučená způsob tooload dat do SQL Data Warehouse pomocí PolyBase je.**</span><span class="sxs-lookup"><span data-stu-id="75f40-213">**Using PolyBase is hello recommended way tooload data into SQL Data Warehouse.**</span></span> <span data-ttu-id="75f40-214">V tématu [použijte PolyBase tooload data do Azure SQL Data Warehouse](#use-polybase-to-load-data-into-azure-sql-data-warehouse) části omezení a podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="75f40-214">See [Use PolyBase tooload data into Azure SQL Data Warehouse](#use-polybase-to-load-data-into-azure-sql-data-warehouse) section for constraints and details.</span></span> |<span data-ttu-id="75f40-215">True</span><span class="sxs-lookup"><span data-stu-id="75f40-215">True</span></span> <br/><span data-ttu-id="75f40-216">NEPRAVDA (výchozí)</span><span class="sxs-lookup"><span data-stu-id="75f40-216">False (default)</span></span> |<span data-ttu-id="75f40-217">Ne</span><span class="sxs-lookup"><span data-stu-id="75f40-217">No</span></span> |
| <span data-ttu-id="75f40-218">polyBaseSettings</span><span class="sxs-lookup"><span data-stu-id="75f40-218">polyBaseSettings</span></span> |<span data-ttu-id="75f40-219">Skupinu vlastností, které je možné zadat při hello **allowPolybase** vlastnost je nastavena příliš**true**.</span><span class="sxs-lookup"><span data-stu-id="75f40-219">A group of properties that can be specified when hello **allowPolybase** property is set too**true**.</span></span> |&nbsp; |<span data-ttu-id="75f40-220">Ne</span><span class="sxs-lookup"><span data-stu-id="75f40-220">No</span></span> |
| <span data-ttu-id="75f40-221">rejectValue</span><span class="sxs-lookup"><span data-stu-id="75f40-221">rejectValue</span></span> |<span data-ttu-id="75f40-222">Určuje číslo hello nebo podíl řádků, které může být odmítnutá před hello se dotaz nezdaří.</span><span class="sxs-lookup"><span data-stu-id="75f40-222">Specifies hello number or percentage of rows that can be rejected before hello query fails.</span></span> <br/><br/><span data-ttu-id="75f40-223">Další informace o hello PolyBase odmítnout možnosti v hello **argumenty** části [vytvořit EXTERNAL TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) tématu.</span><span class="sxs-lookup"><span data-stu-id="75f40-223">Learn more about hello PolyBase’s reject options in hello **Arguments** section of [CREATE EXTERNAL TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) topic.</span></span> |<span data-ttu-id="75f40-224">0 (výchozí), 1, 2...</span><span class="sxs-lookup"><span data-stu-id="75f40-224">0 (default), 1, 2, …</span></span> |<span data-ttu-id="75f40-225">Ne</span><span class="sxs-lookup"><span data-stu-id="75f40-225">No</span></span> |
| <span data-ttu-id="75f40-226">rejectType</span><span class="sxs-lookup"><span data-stu-id="75f40-226">rejectType</span></span> |<span data-ttu-id="75f40-227">Určuje, zda je zadána možnost rejectValue hello literálovou hodnotou nebo jako procento.</span><span class="sxs-lookup"><span data-stu-id="75f40-227">Specifies whether hello rejectValue option is specified as a literal value or a percentage.</span></span> |<span data-ttu-id="75f40-228">Hodnota (výchozí), procento</span><span class="sxs-lookup"><span data-stu-id="75f40-228">Value (default), Percentage</span></span> |<span data-ttu-id="75f40-229">Ne</span><span class="sxs-lookup"><span data-stu-id="75f40-229">No</span></span> |
| <span data-ttu-id="75f40-230">rejectSampleValue</span><span class="sxs-lookup"><span data-stu-id="75f40-230">rejectSampleValue</span></span> |<span data-ttu-id="75f40-231">Určuje hello počet řádků tooretrieve před hello PolyBase přepočítá hello procento odmítnutých řádků.</span><span class="sxs-lookup"><span data-stu-id="75f40-231">Determines hello number of rows tooretrieve before hello PolyBase recalculates hello percentage of rejected rows.</span></span> |<span data-ttu-id="75f40-232">1, 2, …</span><span class="sxs-lookup"><span data-stu-id="75f40-232">1, 2, …</span></span> |<span data-ttu-id="75f40-233">Ano, pokud **rejectType** je **procento**</span><span class="sxs-lookup"><span data-stu-id="75f40-233">Yes, if **rejectType** is **percentage**</span></span> |
| <span data-ttu-id="75f40-234">useTypeDefault</span><span class="sxs-lookup"><span data-stu-id="75f40-234">useTypeDefault</span></span> |<span data-ttu-id="75f40-235">Určuje, jak toohandle chybějící hodnoty v oddělené textové soubory, když PolyBase načítá data z textového souboru hello.</span><span class="sxs-lookup"><span data-stu-id="75f40-235">Specifies how toohandle missing values in delimited text files when PolyBase retrieves data from hello text file.</span></span><br/><br/><span data-ttu-id="75f40-236">Další informace o této vlastnosti z oddílu argumenty hello v [vytvořit EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx).</span><span class="sxs-lookup"><span data-stu-id="75f40-236">Learn more about this property from hello Arguments section in [CREATE EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx).</span></span> |<span data-ttu-id="75f40-237">Hodnota TRUE, False (výchozí)</span><span class="sxs-lookup"><span data-stu-id="75f40-237">True, False (default)</span></span> |<span data-ttu-id="75f40-238">Ne</span><span class="sxs-lookup"><span data-stu-id="75f40-238">No</span></span> |
| <span data-ttu-id="75f40-239">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="75f40-239">writeBatchSize</span></span> |<span data-ttu-id="75f40-240">Pokud velikost vyrovnávací paměti hello dosáhne writeBatchSize vkládá data do tabulky SQL hello</span><span class="sxs-lookup"><span data-stu-id="75f40-240">Inserts data into hello SQL table when hello buffer size reaches writeBatchSize</span></span> |<span data-ttu-id="75f40-241">Celé číslo (počet řádků)</span><span class="sxs-lookup"><span data-stu-id="75f40-241">Integer (number of rows)</span></span> |<span data-ttu-id="75f40-242">Ne (výchozí: 10000)</span><span class="sxs-lookup"><span data-stu-id="75f40-242">No (default: 10000)</span></span> |
| <span data-ttu-id="75f40-243">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="75f40-243">writeBatchTimeout</span></span> |<span data-ttu-id="75f40-244">Doba pro toocomplete operaci vložení dávky hello Počkejte, než vyprší časový limit.</span><span class="sxs-lookup"><span data-stu-id="75f40-244">Wait time for hello batch insert operation toocomplete before it times out.</span></span> |<span data-ttu-id="75f40-245">Časový interval</span><span class="sxs-lookup"><span data-stu-id="75f40-245">timespan</span></span><br/><br/> <span data-ttu-id="75f40-246">Příklad: "00: 30:00" (30 minut).</span><span class="sxs-lookup"><span data-stu-id="75f40-246">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="75f40-247">Ne</span><span class="sxs-lookup"><span data-stu-id="75f40-247">No</span></span> |

#### <a name="sqldwsink-example"></a><span data-ttu-id="75f40-248">Příklad SqlDWSink</span><span class="sxs-lookup"><span data-stu-id="75f40-248">SqlDWSink example</span></span>

```JSON
"sink": {
    "type": "SqlDWSink",
    "allowPolyBase": true
}
```

## <a name="use-polybase-tooload-data-into-azure-sql-data-warehouse"></a><span data-ttu-id="75f40-249">Použijte PolyBase tooload data do Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="75f40-249">Use PolyBase tooload data into Azure SQL Data Warehouse</span></span>
<span data-ttu-id="75f40-250">Pomocí  **[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide)**  je účinný způsob načítání velké množství dat do Azure SQL Data Warehouse s vysokou propustností.</span><span class="sxs-lookup"><span data-stu-id="75f40-250">Using **[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide)** is an efficient way of loading large amount of data into Azure SQL Data Warehouse with high throughput.</span></span> <span data-ttu-id="75f40-251">Velký nárůst hello propustnost můžete zobrazit pomocí PolyBase místo hello výchozí hromadné vložení mechanismus.</span><span class="sxs-lookup"><span data-stu-id="75f40-251">You can see a large gain in hello throughput by using PolyBase instead of hello default BULKINSERT mechanism.</span></span> <span data-ttu-id="75f40-252">V tématu [zkopírujte výkonu referenční číslo](data-factory-copy-activity-performance.md#performance-reference) s podrobné porovnání.</span><span class="sxs-lookup"><span data-stu-id="75f40-252">See [copy performance reference number](data-factory-copy-activity-performance.md#performance-reference) with detailed comparison.</span></span> <span data-ttu-id="75f40-253">Návod s případu použití najdete v tématu [načíst 1 TB do Azure SQL Data Warehouse pomocí Azure Data Factory v části 15 minut](data-factory-load-sql-data-warehouse.md).</span><span class="sxs-lookup"><span data-stu-id="75f40-253">For a walkthrough with a use case, see [Load 1 TB into Azure SQL Data Warehouse under 15 minutes with Azure Data Factory](data-factory-load-sql-data-warehouse.md).</span></span>

* <span data-ttu-id="75f40-254">Pokud je zdroj dat v **objektů Blob v Azure nebo Azure Data Lake Store**a formát hello je kompatibilní s funkcí PolyBase, můžete zkopírovat přímo tooAzure SQL datového skladu pomocí PolyBase.</span><span class="sxs-lookup"><span data-stu-id="75f40-254">If your source data is in **Azure Blob or Azure Data Lake Store**, and hello format is compatible with PolyBase, you can directly copy tooAzure SQL Data Warehouse using PolyBase.</span></span> <span data-ttu-id="75f40-255">V tématu  **[přímé kopírování pomocí PolyBase](#direct-copy-using-polybase)**  s podrobnostmi.</span><span class="sxs-lookup"><span data-stu-id="75f40-255">See **[Direct copy using PolyBase](#direct-copy-using-polybase)** with details.</span></span>
* <span data-ttu-id="75f40-256">Pokud vaše zdrojového úložiště dat a formát není podporován původně polybase, můžete použít hello  **[připravený kopírování pomocí PolyBase](#staged-copy-using-polybase)**  místo toho funkci.</span><span class="sxs-lookup"><span data-stu-id="75f40-256">If your source data store and format is not originally supported by PolyBase, you can use hello **[Staged Copy using PolyBase](#staged-copy-using-polybase)** feature instead.</span></span> <span data-ttu-id="75f40-257">Také poskytuje lepší propustnosti automaticky převod hello dat do formátu kompatibilní s funkcí PolyBase a ukládání hello dat v úložišti objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="75f40-257">It also provides you better throughput by automatically converting hello data into PolyBase-compatible format and storing hello data in Azure Blob storage.</span></span> <span data-ttu-id="75f40-258">Potom načte data do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="75f40-258">It then loads data into SQL Data Warehouse.</span></span>

<span data-ttu-id="75f40-259">Sada hello `allowPolyBase` vlastnost příliš**true** jak ukazuje následující příklad pro Azure Data Factory toouse PolyBase toocopy data do Azure SQL Data Warehouse hello.</span><span class="sxs-lookup"><span data-stu-id="75f40-259">Set hello `allowPolyBase` property too**true** as shown in hello following example for Azure Data Factory toouse PolyBase toocopy data into Azure SQL Data Warehouse.</span></span> <span data-ttu-id="75f40-260">Když nastavíte allowPolyBase tootrue, můžete zadat konkrétní vlastnosti PolyBase pomocí hello `polyBaseSettings` skupina vlastností.</span><span class="sxs-lookup"><span data-stu-id="75f40-260">When you set allowPolyBase tootrue, you can specify PolyBase specific properties using hello `polyBaseSettings` property group.</span></span> <span data-ttu-id="75f40-261">v tématu hello [SqlDWSink](#SqlDWSink) část Podrobnosti o vlastnosti, které můžete použít s polyBaseSettings.</span><span class="sxs-lookup"><span data-stu-id="75f40-261">see hello [SqlDWSink](#SqlDWSink) section for details about properties that you can use with polyBaseSettings.</span></span>

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

### <a name="direct-copy-using-polybase"></a><span data-ttu-id="75f40-262">Přímé kopírování pomocí PolyBase</span><span class="sxs-lookup"><span data-stu-id="75f40-262">Direct copy using PolyBase</span></span>
<span data-ttu-id="75f40-263">SQL Data Warehouse PolyBase přímo podporují objektů Blob v Azure a Azure Data Lake Store (pomocí instanční objekt) jako zdroj a s požadavky na konkrétní soubor formátu.</span><span class="sxs-lookup"><span data-stu-id="75f40-263">SQL Data Warehouse PolyBase directly support Azure Blob and Azure Data Lake Store (using service principal) as source and with specific file format requirements.</span></span> <span data-ttu-id="75f40-264">Pokud vaše zdrojová data splňuje kritéria hello popsaných v této části, můžete zkopírovat přímo ze zdroje dat úložiště tooAzure, které SQL datového skladu pomocí PolyBase.</span><span class="sxs-lookup"><span data-stu-id="75f40-264">If your source data meets hello criteria described in this section, you can directly copy from source data store tooAzure SQL Data Warehouse using PolyBase.</span></span> <span data-ttu-id="75f40-265">Jinak můžete použít [připravený kopírování pomocí PolyBase](#staged-copy-using-polybase).</span><span class="sxs-lookup"><span data-stu-id="75f40-265">Otherwise, you can use [Staged Copy using PolyBase](#staged-copy-using-polybase).</span></span>

> [!TIP]
> <span data-ttu-id="75f40-266">Další informace z toocopy dat z Data Lake Store tooSQL datového skladu efektivně [Azure Data Factory je i jednodušší a pohodlný toouncover přehled o datech, při použití Data Lake Store s SQL Data Warehouse](https://blogs.msdn.microsoft.com/azuredatalake/2017/04/08/azure-data-factory-makes-it-even-easier-and-convenient-to-uncover-insights-from-data-when-using-data-lake-store-with-sql-data-warehouse/).</span><span class="sxs-lookup"><span data-stu-id="75f40-266">toocopy data from Data Lake Store tooSQL Data Warehouse efficiently, learn more from [Azure Data Factory makes it even easier and convenient toouncover insights from data when using Data Lake Store with SQL Data Warehouse](https://blogs.msdn.microsoft.com/azuredatalake/2017/04/08/azure-data-factory-makes-it-even-easier-and-convenient-to-uncover-insights-from-data-when-using-data-lake-store-with-sql-data-warehouse/).</span></span>

<span data-ttu-id="75f40-267">Pokud nejsou splněny požadavky hello, Azure Data Factory zkontroluje hello nastavení a automaticky se vrátí toohello hromadné vložení mechanismus pro přesun dat hello.</span><span class="sxs-lookup"><span data-stu-id="75f40-267">If hello requirements are not met, Azure Data Factory checks hello settings and automatically falls back toohello BULKINSERT mechanism for hello data movement.</span></span>

1. <span data-ttu-id="75f40-268">**Zdroj propojené služby** je typu: **azurestorage** nebo **AzureDataLakeStore s objekt zabezpečení ověřování služby**.</span><span class="sxs-lookup"><span data-stu-id="75f40-268">**Source linked service** is of type: **AzureStorage** or **AzureDataLakeStore with service principal authentication**.</span></span>  
2. <span data-ttu-id="75f40-269">Hello **vstupní datové sady** je typu: **AzureBlob** nebo **AzureDataLakeStore**, a hello typ formátu pod `type` vlastnosti je **OrcFormat** , nebo **TextFormat** s hello následující konfigurace:</span><span class="sxs-lookup"><span data-stu-id="75f40-269">hello **input dataset** is of type: **AzureBlob** or **AzureDataLakeStore**, and hello format type under `type` properties is **OrcFormat**, or **TextFormat** with hello following configurations:</span></span>

   1. <span data-ttu-id="75f40-270">`rowDelimiter`musí být  **\n** .</span><span class="sxs-lookup"><span data-stu-id="75f40-270">`rowDelimiter` must be **\n**.</span></span>
   2. <span data-ttu-id="75f40-271">`nullValue`je nastaven příliš**prázdný řetězec** (""), nebo `treatEmptyAsNull` je nastaven příliš**true**.</span><span class="sxs-lookup"><span data-stu-id="75f40-271">`nullValue` is set too**empty string** (""), or `treatEmptyAsNull` is set too**true**.</span></span>
   3. <span data-ttu-id="75f40-272">`encodingName`je nastaven příliš**znakové sady utf-8**, což je **výchozí** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="75f40-272">`encodingName` is set too**utf-8**, which is **default** value.</span></span>
   4. <span data-ttu-id="75f40-273">`escapeChar`, `quoteChar`, `firstRowAsHeader`, a `skipLineCount` nejsou zadané.</span><span class="sxs-lookup"><span data-stu-id="75f40-273">`escapeChar`, `quoteChar`, `firstRowAsHeader`, and `skipLineCount` are not specified.</span></span>
   5. <span data-ttu-id="75f40-274">`compression`může být **bez komprese**, **GZip**, nebo **Deflate**.</span><span class="sxs-lookup"><span data-stu-id="75f40-274">`compression` can be **no compression**, **GZip**, or **Deflate**.</span></span>

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

3. <span data-ttu-id="75f40-275">Neexistuje žádné `skipHeaderLineCount` nastavení v části **BlobSource** nebo **AzureDataLakeStore** pro hello aktivitu kopírování v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="75f40-275">There is no `skipHeaderLineCount` setting under **BlobSource** or **AzureDataLakeStore** for hello Copy activity in hello pipeline.</span></span>
4. <span data-ttu-id="75f40-276">Neexistuje žádné `sliceIdentifierColumnName` nastavení v části **SqlDWSink** pro hello aktivitu kopírování v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="75f40-276">There is no `sliceIdentifierColumnName` setting under **SqlDWSink** for hello Copy activity in hello pipeline.</span></span> <span data-ttu-id="75f40-277">(PolyBase zaručuje, že se aktualizuje všechna data nebo nic se aktualizuje v jednom spustit.</span><span class="sxs-lookup"><span data-stu-id="75f40-277">(PolyBase guarantees that all data is updated or nothing is updated in a single run.</span></span> <span data-ttu-id="75f40-278">tooachieve **opakovatelnosti**, můžete použít `sqlWriterCleanupScript`).</span><span class="sxs-lookup"><span data-stu-id="75f40-278">tooachieve **repeatability**, you could use `sqlWriterCleanupScript`).</span></span>
5. <span data-ttu-id="75f40-279">Neexistuje žádné `columnMapping` použitá v hello související v aktivitě kopírování.</span><span class="sxs-lookup"><span data-stu-id="75f40-279">There is no `columnMapping` being used in hello associated in Copy activity.</span></span>

### <a name="staged-copy-using-polybase"></a><span data-ttu-id="75f40-280">Dvoufázové instalace kopírování pomocí PolyBase</span><span class="sxs-lookup"><span data-stu-id="75f40-280">Staged Copy using PolyBase</span></span>
<span data-ttu-id="75f40-281">Pokud vaše zdrojová data nesplňuje kritéria hello byla zavedená v předchozí části hello, můžete povolit kopírování dat prostřednictvím dočasné pracovní Azure Blob Storage (úložiště úrovně Premium nemůže být).</span><span class="sxs-lookup"><span data-stu-id="75f40-281">When your source data doesn’t meet hello criteria introduced in hello previous section, you can enable copying data via an interim staging Azure Blob Storage (cannot be Premium Storage).</span></span> <span data-ttu-id="75f40-282">V takovém případě Azure Data Factory automaticky provede transformace na hello data toomeet data formátu požadavky PolyBase a pak použijte PolyBase tooload dat do SQL Data Warehouse a v poslední čištění Vaše dočasná data z úložiště objektů Blob hello.</span><span class="sxs-lookup"><span data-stu-id="75f40-282">In this case, Azure Data Factory automatically performs transformations on hello data toomeet data format requirements of PolyBase, then use PolyBase tooload data into SQL Data Warehouse, and at last clean-up your temp data from hello Blob storage.</span></span> <span data-ttu-id="75f40-283">V tématu [připravený kopie](data-factory-copy-activity-performance.md#staged-copy) informace o tom, jak kopírování dat prostřednictvím pracovní objektů Blob v Azure funguje obecně.</span><span class="sxs-lookup"><span data-stu-id="75f40-283">See [Staged Copy](data-factory-copy-activity-performance.md#staged-copy) for details on how copying data via a staging Azure Blob works in general.</span></span>

> [!NOTE]
> <span data-ttu-id="75f40-284">Při kopírování dat z data místní ukládání do Azure SQL Data Warehouse pomocí PolyBase a pracovní, pokud vaše verze brány pro správu dat je menší než 2.4, prostředí JRE (prostředí Java Runtime) je nutné na bráně počítač, který je použité tootransform zdrojová data do správný formát.</span><span class="sxs-lookup"><span data-stu-id="75f40-284">When copying data from an on-prem data store into Azure SQL Data Warehouse using PolyBase and staging, if your Data Management Gateway version is below 2.4, JRE (Java Runtime Environment) is required on your gateway machine that is used tootransform your source data into proper format.</span></span> <span data-ttu-id="75f40-285">Naznačují, že upgradu vaší brány toohello nejnovější tooavoid této závislosti.</span><span class="sxs-lookup"><span data-stu-id="75f40-285">Suggest you upgrade your gateway toohello latest tooavoid such dependency.</span></span>
>

<span data-ttu-id="75f40-286">toouse tuto funkci, vytvořit [propojená služba Azure Storage](data-factory-azure-blob-connector.md#azure-storage-linked-service) který označuje toohello účet úložiště Azure, který má hello úložiště objektů blob v mezičase, pak zadejte hello `enableStaging` a `stagingSettings` vlastnosti hello aktivity kopírování jak je znázorněno v následujícím kódu hello:</span><span class="sxs-lookup"><span data-stu-id="75f40-286">toouse this feature, create an [Azure Storage linked service](data-factory-azure-blob-connector.md#azure-storage-linked-service) that refers toohello Azure Storage Account that has hello interim blob storage, then specify hello `enableStaging` and `stagingSettings` properties for hello Copy Activity as shown in hello following code:</span></span>

```json
"activities":[  
{
    "name": "Sample copy activity from SQL Server tooSQL Data Warehouse via PolyBase",
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

## <a name="best-practices-when-using-polybase"></a><span data-ttu-id="75f40-287">Osvědčené postupy při používání funkce PolyBase</span><span class="sxs-lookup"><span data-stu-id="75f40-287">Best practices when using PolyBase</span></span>
<span data-ttu-id="75f40-288">Hello následující části obsahují další osvědčené postupy toohello těch, které jsou uvedené v [osvědčené postupy pro Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="75f40-288">hello following sections provide additional best practices toohello ones that are mentioned in [Best practices for Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md).</span></span>

### <a name="required-database-permission"></a><span data-ttu-id="75f40-289">Oprávnění požadovaná databáze</span><span class="sxs-lookup"><span data-stu-id="75f40-289">Required database permission</span></span>
<span data-ttu-id="75f40-290">toouse PolyBase, vyžaduje hello uživatele se má použít tooload dat do SQL Data Warehouse hello [oprávnění "Řízení"](https://msdn.microsoft.com/library/ms191291.aspx) na hello cílovou databázi.</span><span class="sxs-lookup"><span data-stu-id="75f40-290">toouse PolyBase, it requires hello user being used tooload data into SQL Data Warehouse has hello ["CONTROL" permission](https://msdn.microsoft.com/library/ms191291.aspx) on hello target database.</span></span> <span data-ttu-id="75f40-291">Jedním ze způsobů tooachieve, který je tooadd tento uživatel jako člen role "db_owner".</span><span class="sxs-lookup"><span data-stu-id="75f40-291">One way tooachieve that is tooadd that user as a member of "db_owner" role.</span></span> <span data-ttu-id="75f40-292">Zjistěte, jak toodo, pomocí následujících [v této části](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md#authorization).</span><span class="sxs-lookup"><span data-stu-id="75f40-292">Learn how toodo that by following [this section](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md#authorization).</span></span>

### <a name="row-size-and-data-type-limitation"></a><span data-ttu-id="75f40-293">Velikost řádku a datový typ omezení</span><span class="sxs-lookup"><span data-stu-id="75f40-293">Row size and data type limitation</span></span>
<span data-ttu-id="75f40-294">Polybase zatížení jsou omezené tooloading řádků i menší než **1 MB** a nelze načíst tooVARCHR(MAX), NVARCHAR(MAX) nebo VARBINARY(MAX).</span><span class="sxs-lookup"><span data-stu-id="75f40-294">Polybase loads are limited tooloading rows both smaller than **1 MB** and cannot load tooVARCHR(MAX), NVARCHAR(MAX) or VARBINARY(MAX).</span></span> <span data-ttu-id="75f40-295">Odkazovat příliš[zde](../sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md#loads).</span><span class="sxs-lookup"><span data-stu-id="75f40-295">Refer too[here](../sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md#loads).</span></span>

<span data-ttu-id="75f40-296">Pokud máte zdroj dat s řádky o velikosti větší než 1 MB, měli byste toosplit hello zdrojové tabulky svisle do několik malých ty, kde hello největší velikost řádku každého z nich není delší než hello limit.</span><span class="sxs-lookup"><span data-stu-id="75f40-296">If you have source data with rows of size greater than 1 MB, you may want toosplit hello source tables vertically into several small ones where hello largest row size of each of them does not exceed hello limit.</span></span> <span data-ttu-id="75f40-297">Hello menší tabulky mohou být načteny, pak pomocí PolyBase a sloučí v Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="75f40-297">hello smaller tables can then be loaded using PolyBase and merged together in Azure SQL Data Warehouse.</span></span>

### <a name="sql-data-warehouse-resource-class"></a><span data-ttu-id="75f40-298">Třída prostředků SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="75f40-298">SQL Data Warehouse resource class</span></span>
<span data-ttu-id="75f40-299">tooachieve nejlepší možné propustnost, vezměte v úvahu tooassign větší prostředků třída toohello uživatel právě používá tooload dat do SQL Data Warehouse pomocí PolyBase.</span><span class="sxs-lookup"><span data-stu-id="75f40-299">tooachieve best possible throughput, consider tooassign larger resource class toohello user being used tooload data into SQL Data Warehouse via PolyBase.</span></span> <span data-ttu-id="75f40-300">Zjistěte, jak toodo, pomocí následujících [změnit v příkladu třída prostředků uživatele](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).</span><span class="sxs-lookup"><span data-stu-id="75f40-300">Learn how toodo that by following [Change a user resource class example](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).</span></span>

### <a name="tablename-in-azure-sql-data-warehouse"></a><span data-ttu-id="75f40-301">Název tabulky v Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="75f40-301">tableName in Azure SQL Data Warehouse</span></span>
<span data-ttu-id="75f40-302">Hello následující tabulka obsahuje příklady jak toospecify hello **tableName** vlastnost v datové sadě JSON pro různé kombinace názvu schéma a tabulku.</span><span class="sxs-lookup"><span data-stu-id="75f40-302">hello following table provides examples on how toospecify hello **tableName** property in dataset JSON for various combinations of schema and table name.</span></span>

| <span data-ttu-id="75f40-303">Schéma databáze</span><span class="sxs-lookup"><span data-stu-id="75f40-303">DB Schema</span></span> | <span data-ttu-id="75f40-304">Název tabulky</span><span class="sxs-lookup"><span data-stu-id="75f40-304">Table name</span></span> | <span data-ttu-id="75f40-305">hodnotu vlastnosti tableName JSON</span><span class="sxs-lookup"><span data-stu-id="75f40-305">tableName JSON property</span></span> |
| --- | --- | --- |
| <span data-ttu-id="75f40-306">dbo</span><span class="sxs-lookup"><span data-stu-id="75f40-306">dbo</span></span> |<span data-ttu-id="75f40-307">MyTable</span><span class="sxs-lookup"><span data-stu-id="75f40-307">MyTable</span></span> |<span data-ttu-id="75f40-308">MyTable nebo dbo. MyTable nebo [dbo]. [Tabulka]</span><span class="sxs-lookup"><span data-stu-id="75f40-308">MyTable or dbo.MyTable or [dbo].[MyTable]</span></span> |
| <span data-ttu-id="75f40-309">dbo1</span><span class="sxs-lookup"><span data-stu-id="75f40-309">dbo1</span></span> |<span data-ttu-id="75f40-310">MyTable</span><span class="sxs-lookup"><span data-stu-id="75f40-310">MyTable</span></span> |<span data-ttu-id="75f40-311">dbo1. MyTable nebo [dbo1]. [Tabulka]</span><span class="sxs-lookup"><span data-stu-id="75f40-311">dbo1.MyTable or [dbo1].[MyTable]</span></span> |
| <span data-ttu-id="75f40-312">dbo</span><span class="sxs-lookup"><span data-stu-id="75f40-312">dbo</span></span> |<span data-ttu-id="75f40-313">My.Table</span><span class="sxs-lookup"><span data-stu-id="75f40-313">My.Table</span></span> |<span data-ttu-id="75f40-314">[My.Table] nebo [dbo]. [My.Table]</span><span class="sxs-lookup"><span data-stu-id="75f40-314">[My.Table] or [dbo].[My.Table]</span></span> |
| <span data-ttu-id="75f40-315">dbo1</span><span class="sxs-lookup"><span data-stu-id="75f40-315">dbo1</span></span> |<span data-ttu-id="75f40-316">My.Table</span><span class="sxs-lookup"><span data-stu-id="75f40-316">My.Table</span></span> |<span data-ttu-id="75f40-317">[dbo1]. [My.Table]</span><span class="sxs-lookup"><span data-stu-id="75f40-317">[dbo1].[My.Table]</span></span> |

<span data-ttu-id="75f40-318">Pokud se zobrazí následující chyba hello, může to být problém s hello hodnotu, kterou jste zadali pro hello hodnotu vlastnosti tableName.</span><span class="sxs-lookup"><span data-stu-id="75f40-318">If you see hello following error, it could be an issue with hello value you specified for hello tableName property.</span></span> <span data-ttu-id="75f40-319">V tabulce hello najdete hello správný způsob toospecify hodnoty pro vlastnosti JSON hello tableName.</span><span class="sxs-lookup"><span data-stu-id="75f40-319">See hello table for hello correct way toospecify values for hello tableName JSON property.</span></span>  

```
Type=System.Data.SqlClient.SqlException,Message=Invalid object name 'stg.Account_test'.,Source=.Net SqlClient Data Provider
```

### <a name="columns-with-default-values"></a><span data-ttu-id="75f40-320">Sloupce s výchozími hodnotami</span><span class="sxs-lookup"><span data-stu-id="75f40-320">Columns with default values</span></span>
<span data-ttu-id="75f40-321">V současné době PolyBase funkce v datové továrně přijímá pouze hello stejný počet sloupců jako v cílové tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="75f40-321">Currently, PolyBase feature in Data Factory only accepts hello same number of columns as in hello target table.</span></span> <span data-ttu-id="75f40-322">Řekněme, máte tabulku s čtyři sloupce a jeden z nich je definovaná pomocí výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="75f40-322">Say, you have a table with four columns and one of them is defined with a default value.</span></span> <span data-ttu-id="75f40-323">vstupní data Hello stále by měl obsahovat čtyři sloupce.</span><span class="sxs-lookup"><span data-stu-id="75f40-323">hello input data should still contain four columns.</span></span> <span data-ttu-id="75f40-324">Poskytnutí 3sloupce vstupní datové sady povede k chybě podobné toohello následující zprávou:</span><span class="sxs-lookup"><span data-stu-id="75f40-324">Providing a 3-column input dataset would yield an error similar toohello following message:</span></span>

```
All columns of hello table must be specified in hello INSERT BULK statement.
```
<span data-ttu-id="75f40-325">Hodnota NULL je speciální forma výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="75f40-325">NULL value is a special form of default value.</span></span> <span data-ttu-id="75f40-326">Pokud je hello sloupec s možnou hodnotou Null, hello vstupní data (v objektu blob) pro tento sloupec může být prázdná (nelze chybí vstupní datové sady hello).</span><span class="sxs-lookup"><span data-stu-id="75f40-326">If hello column is nullable, hello input data (in blob) for that column could be empty (cannot be missing from hello input dataset).</span></span> <span data-ttu-id="75f40-327">PolyBase vloží hodnotu NULL pro ně hello Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="75f40-327">PolyBase inserts NULL for them in hello Azure SQL Data Warehouse.</span></span>  

## <a name="auto-table-creation"></a><span data-ttu-id="75f40-328">Automaticky vytvářené tabulky</span><span class="sxs-lookup"><span data-stu-id="75f40-328">Auto table creation</span></span>
<span data-ttu-id="75f40-329">Pokud použijete Průvodce kopírováním toocopy dat z SQL serveru nebo Azure SQL Database tooAzure SQL Data Warehouse a hello tabulku, která odpovídá toohello zdrojová tabulka neexistuje v hello cílové úložiště, Data Factory můžete automaticky vytvořit hello tabulku v hello datový sklad pomocí schématu hello zdrojové tabulky.</span><span class="sxs-lookup"><span data-stu-id="75f40-329">If you are using Copy Wizard toocopy data from SQL Server or Azure SQL Database tooAzure SQL Data Warehouse and hello table that corresponds toohello source table does not exist in hello destination store, Data Factory can automatically create hello table in hello data warehouse by using hello source table schema.</span></span>

<span data-ttu-id="75f40-330">Data Factory vytvoří v cílové úložiště hello hello tabulku s hello stejný název v úložišti dat hello zdrojové tabulky.</span><span class="sxs-lookup"><span data-stu-id="75f40-330">Data Factory creates hello table in hello destination store with hello same table name in hello source data store.</span></span> <span data-ttu-id="75f40-331">Hello datové typy pro sloupce jsou zvolen v závislosti na hello následující mapování typu.</span><span class="sxs-lookup"><span data-stu-id="75f40-331">hello data types for columns are chosen based on hello following type mapping.</span></span> <span data-ttu-id="75f40-332">V případě potřeby provede toofix převody typu žádné nekompatibility mezi zdrojovým a cílovým úložišti.</span><span class="sxs-lookup"><span data-stu-id="75f40-332">If needed, it performs type conversions toofix any incompatibilities between source and destination stores.</span></span> <span data-ttu-id="75f40-333">Používá také distribuce kruhové dotazování tabulky.</span><span class="sxs-lookup"><span data-stu-id="75f40-333">It also uses Round Robin table distribution.</span></span>

| <span data-ttu-id="75f40-334">Typ sloupce zdrojové databáze SQL</span><span class="sxs-lookup"><span data-stu-id="75f40-334">Source SQL Database column type</span></span> | <span data-ttu-id="75f40-335">Cílový typ sloupce SQL DW (omezení velikosti)</span><span class="sxs-lookup"><span data-stu-id="75f40-335">Destination SQL DW column type (size limitation)</span></span> |
| --- | --- |
| <span data-ttu-id="75f40-336">celá čísla</span><span class="sxs-lookup"><span data-stu-id="75f40-336">Int</span></span> | <span data-ttu-id="75f40-337">celá čísla</span><span class="sxs-lookup"><span data-stu-id="75f40-337">Int</span></span> |
| <span data-ttu-id="75f40-338">BigInt</span><span class="sxs-lookup"><span data-stu-id="75f40-338">BigInt</span></span> | <span data-ttu-id="75f40-339">BigInt</span><span class="sxs-lookup"><span data-stu-id="75f40-339">BigInt</span></span> |
| <span data-ttu-id="75f40-340">SmallInt</span><span class="sxs-lookup"><span data-stu-id="75f40-340">SmallInt</span></span> | <span data-ttu-id="75f40-341">SmallInt</span><span class="sxs-lookup"><span data-stu-id="75f40-341">SmallInt</span></span> |
| <span data-ttu-id="75f40-342">TinyInt</span><span class="sxs-lookup"><span data-stu-id="75f40-342">TinyInt</span></span> | <span data-ttu-id="75f40-343">TinyInt</span><span class="sxs-lookup"><span data-stu-id="75f40-343">TinyInt</span></span> |
| <span data-ttu-id="75f40-344">Bit</span><span class="sxs-lookup"><span data-stu-id="75f40-344">Bit</span></span> | <span data-ttu-id="75f40-345">Bit</span><span class="sxs-lookup"><span data-stu-id="75f40-345">Bit</span></span> |
| <span data-ttu-id="75f40-346">Decimal</span><span class="sxs-lookup"><span data-stu-id="75f40-346">Decimal</span></span> | <span data-ttu-id="75f40-347">Decimal</span><span class="sxs-lookup"><span data-stu-id="75f40-347">Decimal</span></span> |
| <span data-ttu-id="75f40-348">číselné</span><span class="sxs-lookup"><span data-stu-id="75f40-348">Numeric</span></span> | <span data-ttu-id="75f40-349">Decimal</span><span class="sxs-lookup"><span data-stu-id="75f40-349">Decimal</span></span> |
| <span data-ttu-id="75f40-350">Plovoucí desetinná čárka</span><span class="sxs-lookup"><span data-stu-id="75f40-350">Float</span></span> | <span data-ttu-id="75f40-351">Plovoucí desetinná čárka</span><span class="sxs-lookup"><span data-stu-id="75f40-351">Float</span></span> |
| <span data-ttu-id="75f40-352">peníze</span><span class="sxs-lookup"><span data-stu-id="75f40-352">Money</span></span> | <span data-ttu-id="75f40-353">peníze</span><span class="sxs-lookup"><span data-stu-id="75f40-353">Money</span></span> |
| <span data-ttu-id="75f40-354">Real</span><span class="sxs-lookup"><span data-stu-id="75f40-354">Real</span></span> | <span data-ttu-id="75f40-355">Real</span><span class="sxs-lookup"><span data-stu-id="75f40-355">Real</span></span> |
| <span data-ttu-id="75f40-356">SmallMoney</span><span class="sxs-lookup"><span data-stu-id="75f40-356">SmallMoney</span></span> | <span data-ttu-id="75f40-357">SmallMoney</span><span class="sxs-lookup"><span data-stu-id="75f40-357">SmallMoney</span></span> |
| <span data-ttu-id="75f40-358">Binární</span><span class="sxs-lookup"><span data-stu-id="75f40-358">Binary</span></span> | <span data-ttu-id="75f40-359">Binární</span><span class="sxs-lookup"><span data-stu-id="75f40-359">Binary</span></span> |
| <span data-ttu-id="75f40-360">varbinary</span><span class="sxs-lookup"><span data-stu-id="75f40-360">Varbinary</span></span> | <span data-ttu-id="75f40-361">Varbinary (až too8000)</span><span class="sxs-lookup"><span data-stu-id="75f40-361">Varbinary (up too8000)</span></span> |
| <span data-ttu-id="75f40-362">Datum</span><span class="sxs-lookup"><span data-stu-id="75f40-362">Date</span></span> | <span data-ttu-id="75f40-363">Datum</span><span class="sxs-lookup"><span data-stu-id="75f40-363">Date</span></span> |
| <span data-ttu-id="75f40-364">Data a času</span><span class="sxs-lookup"><span data-stu-id="75f40-364">DateTime</span></span> | <span data-ttu-id="75f40-365">Data a času</span><span class="sxs-lookup"><span data-stu-id="75f40-365">DateTime</span></span> |
| <span data-ttu-id="75f40-366">DateTime2</span><span class="sxs-lookup"><span data-stu-id="75f40-366">DateTime2</span></span> | <span data-ttu-id="75f40-367">DateTime2</span><span class="sxs-lookup"><span data-stu-id="75f40-367">DateTime2</span></span> |
| <span data-ttu-id="75f40-368">Čas</span><span class="sxs-lookup"><span data-stu-id="75f40-368">Time</span></span> | <span data-ttu-id="75f40-369">Čas</span><span class="sxs-lookup"><span data-stu-id="75f40-369">Time</span></span> |
| <span data-ttu-id="75f40-370">Datový typ DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="75f40-370">DateTimeOffset</span></span> | <span data-ttu-id="75f40-371">Datový typ DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="75f40-371">DateTimeOffset</span></span> |
| <span data-ttu-id="75f40-372">SmallDateTime</span><span class="sxs-lookup"><span data-stu-id="75f40-372">SmallDateTime</span></span> | <span data-ttu-id="75f40-373">SmallDateTime</span><span class="sxs-lookup"><span data-stu-id="75f40-373">SmallDateTime</span></span> |
| <span data-ttu-id="75f40-374">Text</span><span class="sxs-lookup"><span data-stu-id="75f40-374">Text</span></span> | <span data-ttu-id="75f40-375">Varchar (až too8000)</span><span class="sxs-lookup"><span data-stu-id="75f40-375">Varchar (up too8000)</span></span> |
| <span data-ttu-id="75f40-376">NText</span><span class="sxs-lookup"><span data-stu-id="75f40-376">NText</span></span> | <span data-ttu-id="75f40-377">NVarChar (až too4000)</span><span class="sxs-lookup"><span data-stu-id="75f40-377">NVarChar (up too4000)</span></span> |
| <span data-ttu-id="75f40-378">Image</span><span class="sxs-lookup"><span data-stu-id="75f40-378">Image</span></span> | <span data-ttu-id="75f40-379">VarBinary (až too8000)</span><span class="sxs-lookup"><span data-stu-id="75f40-379">VarBinary (up too8000)</span></span> |
| <span data-ttu-id="75f40-380">Typ UniqueIdentifier</span><span class="sxs-lookup"><span data-stu-id="75f40-380">UniqueIdentifier</span></span> | <span data-ttu-id="75f40-381">Typ UniqueIdentifier</span><span class="sxs-lookup"><span data-stu-id="75f40-381">UniqueIdentifier</span></span> |
| <span data-ttu-id="75f40-382">Char</span><span class="sxs-lookup"><span data-stu-id="75f40-382">Char</span></span> | <span data-ttu-id="75f40-383">Char</span><span class="sxs-lookup"><span data-stu-id="75f40-383">Char</span></span> |
| <span data-ttu-id="75f40-384">NChar</span><span class="sxs-lookup"><span data-stu-id="75f40-384">NChar</span></span> | <span data-ttu-id="75f40-385">NChar</span><span class="sxs-lookup"><span data-stu-id="75f40-385">NChar</span></span> |
| <span data-ttu-id="75f40-386">VarChar</span><span class="sxs-lookup"><span data-stu-id="75f40-386">VarChar</span></span> | <span data-ttu-id="75f40-387">VarChar (až too8000)</span><span class="sxs-lookup"><span data-stu-id="75f40-387">VarChar (up too8000)</span></span> |
| <span data-ttu-id="75f40-388">NVarChar</span><span class="sxs-lookup"><span data-stu-id="75f40-388">NVarChar</span></span> | <span data-ttu-id="75f40-389">NVarChar (až too4000)</span><span class="sxs-lookup"><span data-stu-id="75f40-389">NVarChar (up too4000)</span></span> |
| <span data-ttu-id="75f40-390">XML</span><span class="sxs-lookup"><span data-stu-id="75f40-390">Xml</span></span> | <span data-ttu-id="75f40-391">Varchar (až too8000)</span><span class="sxs-lookup"><span data-stu-id="75f40-391">Varchar (up too8000)</span></span> |

[!INCLUDE [data-factory-type-repeatability-for-sql-sources](../../includes/data-factory-type-repeatability-for-sql-sources.md)]

## <a name="type-mapping-for-azure-sql-data-warehouse"></a><span data-ttu-id="75f40-392">Mapování typu pro Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="75f40-392">Type mapping for Azure SQL Data Warehouse</span></span>
<span data-ttu-id="75f40-393">Jak je uvedeno v hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů toosink typy zdroje s hello následující přístup krok 2:</span><span class="sxs-lookup"><span data-stu-id="75f40-393">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types toosink types with hello following 2-step approach:</span></span>

1. <span data-ttu-id="75f40-394">Převod z typu too.NET typy nativní zdroje</span><span class="sxs-lookup"><span data-stu-id="75f40-394">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="75f40-395">Převést typ jímky toonative typ rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="75f40-395">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="75f40-396">Při přesouvání dat příliš & z Azure SQL Data Warehouse, hello následující mapování se používají z typu too.NET typ SQL a naopak.</span><span class="sxs-lookup"><span data-stu-id="75f40-396">When moving data too& from Azure SQL Data Warehouse, hello following mappings are used from SQL type too.NET type and vice versa.</span></span>

<span data-ttu-id="75f40-397">mapování Hello je stejný jako hello [mapování datového typu aplikace SQL Server pro technologii ADO.NET](https://msdn.microsoft.com/library/cc716729.aspx).</span><span class="sxs-lookup"><span data-stu-id="75f40-397">hello mapping is same as hello [SQL Server Data Type Mapping for ADO.NET](https://msdn.microsoft.com/library/cc716729.aspx).</span></span>

| <span data-ttu-id="75f40-398">Typ databázového stroje SQL Server</span><span class="sxs-lookup"><span data-stu-id="75f40-398">SQL Server Database Engine type</span></span> | <span data-ttu-id="75f40-399">Typ rozhraní .NET framework</span><span class="sxs-lookup"><span data-stu-id="75f40-399">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="75f40-400">bigint</span><span class="sxs-lookup"><span data-stu-id="75f40-400">bigint</span></span> |<span data-ttu-id="75f40-401">Int64</span><span class="sxs-lookup"><span data-stu-id="75f40-401">Int64</span></span> |
| <span data-ttu-id="75f40-402">Binární</span><span class="sxs-lookup"><span data-stu-id="75f40-402">binary</span></span> |<span data-ttu-id="75f40-403">Byte]</span><span class="sxs-lookup"><span data-stu-id="75f40-403">Byte[]</span></span> |
| <span data-ttu-id="75f40-404">Bit</span><span class="sxs-lookup"><span data-stu-id="75f40-404">bit</span></span> |<span data-ttu-id="75f40-405">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="75f40-405">Boolean</span></span> |
| <span data-ttu-id="75f40-406">Char</span><span class="sxs-lookup"><span data-stu-id="75f40-406">char</span></span> |<span data-ttu-id="75f40-407">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="75f40-407">String, Char[]</span></span> |
| <span data-ttu-id="75f40-408">Datum</span><span class="sxs-lookup"><span data-stu-id="75f40-408">date</span></span> |<span data-ttu-id="75f40-409">Data a času</span><span class="sxs-lookup"><span data-stu-id="75f40-409">DateTime</span></span> |
| <span data-ttu-id="75f40-410">Data a času</span><span class="sxs-lookup"><span data-stu-id="75f40-410">Datetime</span></span> |<span data-ttu-id="75f40-411">Data a času</span><span class="sxs-lookup"><span data-stu-id="75f40-411">DateTime</span></span> |
| <span data-ttu-id="75f40-412">datetime2</span><span class="sxs-lookup"><span data-stu-id="75f40-412">datetime2</span></span> |<span data-ttu-id="75f40-413">Data a času</span><span class="sxs-lookup"><span data-stu-id="75f40-413">DateTime</span></span> |
| <span data-ttu-id="75f40-414">Datový typ DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="75f40-414">Datetimeoffset</span></span> |<span data-ttu-id="75f40-415">Datový typ DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="75f40-415">DateTimeOffset</span></span> |
| <span data-ttu-id="75f40-416">Decimal</span><span class="sxs-lookup"><span data-stu-id="75f40-416">Decimal</span></span> |<span data-ttu-id="75f40-417">Decimal</span><span class="sxs-lookup"><span data-stu-id="75f40-417">Decimal</span></span> |
| <span data-ttu-id="75f40-418">Atribut FILESTREAM (varbinary(max))</span><span class="sxs-lookup"><span data-stu-id="75f40-418">FILESTREAM attribute (varbinary(max))</span></span> |<span data-ttu-id="75f40-419">Byte]</span><span class="sxs-lookup"><span data-stu-id="75f40-419">Byte[]</span></span> |
| <span data-ttu-id="75f40-420">Plovoucí desetinná čárka</span><span class="sxs-lookup"><span data-stu-id="75f40-420">Float</span></span> |<span data-ttu-id="75f40-421">Double</span><span class="sxs-lookup"><span data-stu-id="75f40-421">Double</span></span> |
| <span data-ttu-id="75f40-422">Bitové kopie</span><span class="sxs-lookup"><span data-stu-id="75f40-422">image</span></span> |<span data-ttu-id="75f40-423">Byte]</span><span class="sxs-lookup"><span data-stu-id="75f40-423">Byte[]</span></span> |
| <span data-ttu-id="75f40-424">celá čísla</span><span class="sxs-lookup"><span data-stu-id="75f40-424">int</span></span> |<span data-ttu-id="75f40-425">Int32</span><span class="sxs-lookup"><span data-stu-id="75f40-425">Int32</span></span> |
| <span data-ttu-id="75f40-426">peníze</span><span class="sxs-lookup"><span data-stu-id="75f40-426">money</span></span> |<span data-ttu-id="75f40-427">Decimal</span><span class="sxs-lookup"><span data-stu-id="75f40-427">Decimal</span></span> |
| <span data-ttu-id="75f40-428">nchar</span><span class="sxs-lookup"><span data-stu-id="75f40-428">nchar</span></span> |<span data-ttu-id="75f40-429">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="75f40-429">String, Char[]</span></span> |
| <span data-ttu-id="75f40-430">ntext</span><span class="sxs-lookup"><span data-stu-id="75f40-430">ntext</span></span> |<span data-ttu-id="75f40-431">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="75f40-431">String, Char[]</span></span> |
| <span data-ttu-id="75f40-432">číselné</span><span class="sxs-lookup"><span data-stu-id="75f40-432">numeric</span></span> |<span data-ttu-id="75f40-433">Decimal</span><span class="sxs-lookup"><span data-stu-id="75f40-433">Decimal</span></span> |
| <span data-ttu-id="75f40-434">nvarchar</span><span class="sxs-lookup"><span data-stu-id="75f40-434">nvarchar</span></span> |<span data-ttu-id="75f40-435">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="75f40-435">String, Char[]</span></span> |
| <span data-ttu-id="75f40-436">skutečné</span><span class="sxs-lookup"><span data-stu-id="75f40-436">real</span></span> |<span data-ttu-id="75f40-437">Jeden</span><span class="sxs-lookup"><span data-stu-id="75f40-437">Single</span></span> |
| <span data-ttu-id="75f40-438">ROWVERSION</span><span class="sxs-lookup"><span data-stu-id="75f40-438">rowversion</span></span> |<span data-ttu-id="75f40-439">Byte]</span><span class="sxs-lookup"><span data-stu-id="75f40-439">Byte[]</span></span> |
| <span data-ttu-id="75f40-440">smalldatetime</span><span class="sxs-lookup"><span data-stu-id="75f40-440">smalldatetime</span></span> |<span data-ttu-id="75f40-441">Data a času</span><span class="sxs-lookup"><span data-stu-id="75f40-441">DateTime</span></span> |
| <span data-ttu-id="75f40-442">smallint</span><span class="sxs-lookup"><span data-stu-id="75f40-442">smallint</span></span> |<span data-ttu-id="75f40-443">Int16</span><span class="sxs-lookup"><span data-stu-id="75f40-443">Int16</span></span> |
| <span data-ttu-id="75f40-444">Smallmoney</span><span class="sxs-lookup"><span data-stu-id="75f40-444">smallmoney</span></span> |<span data-ttu-id="75f40-445">Decimal</span><span class="sxs-lookup"><span data-stu-id="75f40-445">Decimal</span></span> |
| <span data-ttu-id="75f40-446">SQL_VARIANT</span><span class="sxs-lookup"><span data-stu-id="75f40-446">sql_variant</span></span> |<span data-ttu-id="75f40-447">Objekt *</span><span class="sxs-lookup"><span data-stu-id="75f40-447">Object *</span></span> |
| <span data-ttu-id="75f40-448">Text</span><span class="sxs-lookup"><span data-stu-id="75f40-448">text</span></span> |<span data-ttu-id="75f40-449">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="75f40-449">String, Char[]</span></span> |
| <span data-ttu-id="75f40-450">time</span><span class="sxs-lookup"><span data-stu-id="75f40-450">time</span></span> |<span data-ttu-id="75f40-451">Časový interval</span><span class="sxs-lookup"><span data-stu-id="75f40-451">TimeSpan</span></span> |
| <span data-ttu-id="75f40-452">časové razítko</span><span class="sxs-lookup"><span data-stu-id="75f40-452">timestamp</span></span> |<span data-ttu-id="75f40-453">Byte]</span><span class="sxs-lookup"><span data-stu-id="75f40-453">Byte[]</span></span> |
| <span data-ttu-id="75f40-454">tinyint</span><span class="sxs-lookup"><span data-stu-id="75f40-454">tinyint</span></span> |<span data-ttu-id="75f40-455">Bajtů</span><span class="sxs-lookup"><span data-stu-id="75f40-455">Byte</span></span> |
| <span data-ttu-id="75f40-456">Typ UniqueIdentifier</span><span class="sxs-lookup"><span data-stu-id="75f40-456">uniqueidentifier</span></span> |<span data-ttu-id="75f40-457">Identifikátor GUID</span><span class="sxs-lookup"><span data-stu-id="75f40-457">Guid</span></span> |
| <span data-ttu-id="75f40-458">varbinary</span><span class="sxs-lookup"><span data-stu-id="75f40-458">varbinary</span></span> |<span data-ttu-id="75f40-459">Byte]</span><span class="sxs-lookup"><span data-stu-id="75f40-459">Byte[]</span></span> |
| <span data-ttu-id="75f40-460">varchar</span><span class="sxs-lookup"><span data-stu-id="75f40-460">varchar</span></span> |<span data-ttu-id="75f40-461">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="75f40-461">String, Char[]</span></span> |
| <span data-ttu-id="75f40-462">xml</span><span class="sxs-lookup"><span data-stu-id="75f40-462">xml</span></span> |<span data-ttu-id="75f40-463">XML</span><span class="sxs-lookup"><span data-stu-id="75f40-463">Xml</span></span> |

<span data-ttu-id="75f40-464">Můžete také mapovat sloupců z toocolumns datové sady zdroje z podřízený datovou sadu v definici aktivity kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="75f40-464">You can also map columns from source dataset toocolumns from sink dataset in hello copy activity definition.</span></span> <span data-ttu-id="75f40-465">Podrobnosti najdete v tématu [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="75f40-465">For details, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="json-examples-for-copying-data-tooand-from-sql-data-warehouse"></a><span data-ttu-id="75f40-466">Příklady JSON pro kopírování tooand dat z SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="75f40-466">JSON examples for copying data tooand from SQL Data Warehouse</span></span>
<span data-ttu-id="75f40-467">Hello následující příklady poskytují definice JSON ukázka používané toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="75f40-467">hello following examples provide sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="75f40-468">Ukazují jak toocopy tooand dat z Azure SQL Data Warehouse a Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="75f40-468">They show how toocopy data tooand from Azure SQL Data Warehouse and Azure Blob Storage.</span></span> <span data-ttu-id="75f40-469">Nicméně je možné zkopírovat data **přímo** ze všech zdrojů tooany z hello jímky uvádí [zde](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="75f40-469">However, data can be copied **directly** from any of sources tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>

### <a name="example-copy-data-from-azure-sql-data-warehouse-tooazure-blob"></a><span data-ttu-id="75f40-470">Příklad: Kopírování dat z Azure SQL Data Warehouse tooAzure objektů Blob</span><span class="sxs-lookup"><span data-stu-id="75f40-470">Example: Copy data from Azure SQL Data Warehouse tooAzure Blob</span></span>
<span data-ttu-id="75f40-471">Ukázka Hello definuje hello následující entity služby Data Factory:</span><span class="sxs-lookup"><span data-stu-id="75f40-471">hello sample defines hello following Data Factory entities:</span></span>

1. <span data-ttu-id="75f40-472">Propojené služby typu [AzureSqlDW](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="75f40-472">A linked service of type [AzureSqlDW](#linked-service-properties).</span></span>
2. <span data-ttu-id="75f40-473">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="75f40-473">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="75f40-474">Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureSqlDWTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="75f40-474">An input [dataset](data-factory-create-datasets.md) of type [AzureSqlDWTable](#dataset-properties).</span></span>
4. <span data-ttu-id="75f40-475">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="75f40-475">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="75f40-476">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [SqlDWSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="75f40-476">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [SqlDWSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="75f40-477">Ukázka Hello zkopíruje data časové řady (hodinové, denní, atd.) z tabulky v objektu blob tooa databáze Azure SQL Data Warehouse každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="75f40-477">hello sample copies time-series (hourly, daily, etc.) data from a table in Azure SQL Data Warehouse database tooa blob every hour.</span></span> <span data-ttu-id="75f40-478">Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.</span><span class="sxs-lookup"><span data-stu-id="75f40-478">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="75f40-479">**Azure SQL Data Warehouse propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="75f40-479">**Azure SQL Data Warehouse linked service:**</span></span>

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
<span data-ttu-id="75f40-480">**Propojená služba Azure Blob storage:**</span><span class="sxs-lookup"><span data-stu-id="75f40-480">**Azure Blob storage linked service:**</span></span>

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
<span data-ttu-id="75f40-481">**Azure SQL Data Warehouse vstupní datové sady:**</span><span class="sxs-lookup"><span data-stu-id="75f40-481">**Azure SQL Data Warehouse input dataset:**</span></span>

<span data-ttu-id="75f40-482">Ukázka Hello předpokládá jste vytvořili tabulku "MyTable" v Azure SQL Data Warehouse a obsahuje sloupec s názvem "timestampcolumn" pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="75f40-482">hello sample assumes you have created a table “MyTable” in Azure SQL Data Warehouse and it contains a column called “timestampcolumn” for time series data.</span></span>

<span data-ttu-id="75f40-483">Nastavení "externí": "PRAVDA" informuje služba Data Factory hello tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="75f40-483">Setting “external”: ”true” informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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
<span data-ttu-id="75f40-484">**Azure Blob výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="75f40-484">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="75f40-485">Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="75f40-485">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="75f40-486">Cesta ke složce Hello pro objekt blob hello je vyhodnocován dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="75f40-486">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="75f40-487">Cesta ke složce Hello používá rok, měsíc, den a čas části hello počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="75f40-487">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

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

<span data-ttu-id="75f40-488">**Aktivita kopírování v kanálu s SqlDWSource a BlobSink:**</span><span class="sxs-lookup"><span data-stu-id="75f40-488">**Copy activity in a pipeline with SqlDWSource and BlobSink:**</span></span>

<span data-ttu-id="75f40-489">Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="75f40-489">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="75f40-490">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**SqlDWSource** a **podřízený** je typ nastaven příliš**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="75f40-490">In hello pipeline JSON definition, hello **source** type is set too**SqlDWSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="75f40-491">Dotaz SQL Hello zadaný pro hello **SqlReaderQuery** vlastnost vybere hello data v hello za hodinu toocopy.</span><span class="sxs-lookup"><span data-stu-id="75f40-491">hello SQL query specified for hello **SqlReaderQuery** property selects hello data in hello past hour toocopy.</span></span>

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
> <span data-ttu-id="75f40-492">V příkladu hello **sqlReaderQuery** pro hello SqlDWSource je zadána.</span><span class="sxs-lookup"><span data-stu-id="75f40-492">In hello example, **sqlReaderQuery** is specified for hello SqlDWSource.</span></span> <span data-ttu-id="75f40-493">Aktivita kopírování Hello spouští tento dotaz hello Azure SQL Data Warehouse zdrojová tooget hello data.</span><span class="sxs-lookup"><span data-stu-id="75f40-493">hello Copy Activity runs this query against hello Azure SQL Data Warehouse source tooget hello data.</span></span>
>
> <span data-ttu-id="75f40-494">Alternativně můžete zadat uložené procedury zadáním hello **sqlReaderStoredProcedureName** a **storedProcedureParameters** (Pokud hello uložená procedura používá parametry).</span><span class="sxs-lookup"><span data-stu-id="75f40-494">Alternatively, you can specify a stored procedure by specifying hello **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if hello stored procedure takes parameters).</span></span>
>
> <span data-ttu-id="75f40-495">Pokud nezadáte sqlReaderQuery nebo sqlReaderStoredProcedureName, hello sloupce definované v části struktura hello hello sady dat JSON jsou použité toobuild dotazu (vyberte Sloupec1, Sloupec2 z mytable) toorun proti hello Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="75f40-495">If you do not specify either sqlReaderQuery or sqlReaderStoredProcedureName, hello columns defined in hello structure section of hello dataset JSON are used toobuild a query (select column1, column2 from mytable) toorun against hello Azure SQL Data Warehouse.</span></span> <span data-ttu-id="75f40-496">Pokud definice datové sady hello nemá hello struktura, vyberou se všechny sloupce z tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="75f40-496">If hello dataset definition does not have hello structure, all columns are selected from hello table.</span></span>
>
>

### <a name="example-copy-data-from-azure-blob-tooazure-sql-data-warehouse"></a><span data-ttu-id="75f40-497">Příklad: Kopírování dat z Azure Blob tooAzure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="75f40-497">Example: Copy data from Azure Blob tooAzure SQL Data Warehouse</span></span>
<span data-ttu-id="75f40-498">Ukázka Hello definuje hello následující entity služby Data Factory:</span><span class="sxs-lookup"><span data-stu-id="75f40-498">hello sample defines hello following Data Factory entities:</span></span>

1. <span data-ttu-id="75f40-499">Propojené služby typu [AzureSqlDW](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="75f40-499">A linked service of type [AzureSqlDW](#linked-service-properties).</span></span>
2. <span data-ttu-id="75f40-500">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="75f40-500">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="75f40-501">Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="75f40-501">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="75f40-502">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureSqlDWTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="75f40-502">An output [dataset](data-factory-create-datasets.md) of type [AzureSqlDWTable](#dataset-properties).</span></span>
5. <span data-ttu-id="75f40-503">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) a [SqlDWSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="75f40-503">A [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) and [SqlDWSink](#copy-activity-properties).</span></span>

<span data-ttu-id="75f40-504">Ukázka Hello zkopíruje data časové řady (hodinový, denní, atd.) z Azure blob tooa tabulky v databázi Azure SQL Data Warehouse každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="75f40-504">hello sample copies time-series data (hourly, daily, etc.) from Azure blob tooa table in Azure SQL Data Warehouse database every hour.</span></span> <span data-ttu-id="75f40-505">Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.</span><span class="sxs-lookup"><span data-stu-id="75f40-505">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="75f40-506">**Azure SQL Data Warehouse propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="75f40-506">**Azure SQL Data Warehouse linked service:**</span></span>

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
<span data-ttu-id="75f40-507">**Propojená služba Azure Blob storage:**</span><span class="sxs-lookup"><span data-stu-id="75f40-507">**Azure Blob storage linked service:**</span></span>

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
<span data-ttu-id="75f40-508">**Azure vstupní datovou sadu objektu Blob:**</span><span class="sxs-lookup"><span data-stu-id="75f40-508">**Azure Blob input dataset:**</span></span>

<span data-ttu-id="75f40-509">Data je převzata z nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="75f40-509">Data is picked up from a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="75f40-510">název a cesta k souboru složky Hello pro objekt blob hello se vyhodnocují dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="75f40-510">hello folder path and file name for hello blob are dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="75f40-511">Cesta ke složce Hello používá rok, měsíc a den součástí hello počáteční čas a název souboru používá hello hodinu součástí hello počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="75f40-511">hello folder path uses year, month, and day part of hello start time and file name uses hello hour part of hello start time.</span></span> <span data-ttu-id="75f40-512">"externí": "PRAVDA" nastavení informuje hello služba Data Factory, tato tabulka je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="75f40-512">“external”: “true” setting informs hello Data Factory service that this table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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
<span data-ttu-id="75f40-513">**Azure SQL Data Warehouse výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="75f40-513">**Azure SQL Data Warehouse output dataset:**</span></span>

<span data-ttu-id="75f40-514">Ukázka Hello zkopíruje data tooa tabulku s názvem "MyTable" v Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="75f40-514">hello sample copies data tooa table named “MyTable” in Azure SQL Data Warehouse.</span></span> <span data-ttu-id="75f40-515">Vytvoření tabulky hello v Azure SQL Data Warehouse s hello stejný počet sloupců, podle očekávání toocontain soubor Blob CSV hello.</span><span class="sxs-lookup"><span data-stu-id="75f40-515">Create hello table in Azure SQL Data Warehouse with hello same number of columns as you expect hello Blob CSV file toocontain.</span></span> <span data-ttu-id="75f40-516">Přidávání řádků tabulky toohello každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="75f40-516">New rows are added toohello table every hour.</span></span>

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
<span data-ttu-id="75f40-517">**Aktivita kopírování v kanálu s BlobSource a SqlDWSink:**</span><span class="sxs-lookup"><span data-stu-id="75f40-517">**Copy activity in a pipeline with BlobSource and SqlDWSink:**</span></span>

<span data-ttu-id="75f40-518">Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="75f40-518">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="75f40-519">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**BlobSource** a **podřízený** je typ nastaven příliš**SqlDWSink**.</span><span class="sxs-lookup"><span data-stu-id="75f40-519">In hello pipeline JSON definition, hello **source** type is set too**BlobSource** and **sink** type is set too**SqlDWSink**.</span></span>

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
<span data-ttu-id="75f40-520">Podrobný postup najdete v části hello najdete v části [načíst 1 TB do Azure SQL Data Warehouse pomocí Azure Data Factory v části 15 minut](data-factory-load-sql-data-warehouse.md) a [načtení dat pomocí Azure Data Factory](../sql-data-warehouse/sql-data-warehouse-get-started-load-with-azure-data-factory.md) článek v hello Azure SQL Data Warehouse dokumentace.</span><span class="sxs-lookup"><span data-stu-id="75f40-520">For a walkthrough, see hello see [Load 1 TB into Azure SQL Data Warehouse under 15 minutes with Azure Data Factory](data-factory-load-sql-data-warehouse.md) and [Load data with Azure Data Factory](../sql-data-warehouse/sql-data-warehouse-get-started-load-with-azure-data-factory.md) article in hello Azure SQL Data Warehouse documentation.</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="75f40-521">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="75f40-521">Performance and Tuning</span></span>
<span data-ttu-id="75f40-522">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.</span><span class="sxs-lookup"><span data-stu-id="75f40-522">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
