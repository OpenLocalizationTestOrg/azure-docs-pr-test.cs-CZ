---
title: "Kopírování dat do/z Azure SQL Database | Microsoft Docs"
description: "Zjistěte, jak ke zkopírování dat z Azure SQL Database pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 484f735b-8464-40ba-a9fc-820e6553159e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: a64d13fa7dc5f50c259b98774be80b603dce400a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="copy-data-to-and-from-azure-sql-database-using-azure-data-factory"></a><span data-ttu-id="489d9-103">Kopírování dat do a z Azure SQL Database pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="489d9-103">Copy data to and from Azure SQL Database using Azure Data Factory</span></span>
<span data-ttu-id="489d9-104">Tento článek vysvětluje, jak pomocí aktivity kopírování v Azure Data Factory pro přesun dat do a z Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="489d9-104">This article explains how to use the Copy Activity in Azure Data Factory to move data to and from Azure SQL Database.</span></span> <span data-ttu-id="489d9-105">Vychází [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="489d9-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>  

## <a name="supported-scenarios"></a><span data-ttu-id="489d9-106">Podporované scénáře</span><span class="sxs-lookup"><span data-stu-id="489d9-106">Supported scenarios</span></span>
<span data-ttu-id="489d9-107">Může kopírovat data **z databáze Azure SQL Database** ukládá do následující data:</span><span class="sxs-lookup"><span data-stu-id="489d9-107">You can copy data **from Azure SQL Database** to the following data stores:</span></span>

[!INCLUDE [data-factory-supported-sinks](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="489d9-108">Může kopírovat data z následujících datových úložišť **do Azure SQL Database**:</span><span class="sxs-lookup"><span data-stu-id="489d9-108">You can copy data from the following data stores **to Azure SQL Database**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

## <a name="supported-authentication-type"></a><span data-ttu-id="489d9-109">Podporované typ ověřování</span><span class="sxs-lookup"><span data-stu-id="489d9-109">Supported authentication type</span></span>
<span data-ttu-id="489d9-110">Konektor služby Azure SQL Database podporuje základní ověřování.</span><span class="sxs-lookup"><span data-stu-id="489d9-110">Azure SQL Database connector supports basic authentication.</span></span>

## <a name="getting-started"></a><span data-ttu-id="489d9-111">Začínáme</span><span class="sxs-lookup"><span data-stu-id="489d9-111">Getting started</span></span>
<span data-ttu-id="489d9-112">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z Azure SQL Database pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="489d9-112">You can create a pipeline with a copy activity that moves data to/from an Azure SQL Database by using different tools/APIs.</span></span>

<span data-ttu-id="489d9-113">Nejjednodušší způsob, jak vytvořit kanál je použití **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="489d9-113">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="489d9-114">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírováním data.</span><span class="sxs-lookup"><span data-stu-id="489d9-114">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="489d9-115">Tyto nástroje můžete také použít k vytvoření kanálu: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru**, **.NET API**, a **REST API**.</span><span class="sxs-lookup"><span data-stu-id="489d9-115">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="489d9-116">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="489d9-116">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="489d9-117">Jestli používáte nástroje nebo rozhraní API, je třeba provést následující kroky k vytvoření kanálu, který přesouvá data ze zdrojového úložiště dat do úložiště dat podřízený:</span><span class="sxs-lookup"><span data-stu-id="489d9-117">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span> 

1. <span data-ttu-id="489d9-118">Vytvoření **objekt pro vytváření dat**.</span><span class="sxs-lookup"><span data-stu-id="489d9-118">Create a **data factory**.</span></span> <span data-ttu-id="489d9-119">Objekt pro vytváření dat může obsahovat jeden nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="489d9-119">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="489d9-120">Vytvoření **propojené služby** propojení vstupní a výstupní data ukládá do data factory.</span><span class="sxs-lookup"><span data-stu-id="489d9-120">Create **linked services** to link input and output data stores to your data factory.</span></span> <span data-ttu-id="489d9-121">Pokud jsou kopírování dat z Azure blob storage do Azure SQL database, například vytvoříte dvě propojené služby pro vytváření dat. propojení účtu úložiště Azure a Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="489d9-121">For example, if you are copying data from an Azure blob storage to an Azure SQL database, you create two linked services to link your Azure storage account and Azure SQL database to your data factory.</span></span> <span data-ttu-id="489d9-122">Vlastnosti propojené služby, které jsou specifické pro Azure SQL Database, najdete v části [propojené vlastnosti služby](#linked-service-properties) části.</span><span class="sxs-lookup"><span data-stu-id="489d9-122">For linked service properties that are specific to Azure SQL Database, see [linked service properties](#linked-service-properties) section.</span></span> 
3. <span data-ttu-id="489d9-123">Vytvoření **datové sady** představují vstupní a výstupní data pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="489d9-123">Create **datasets** to represent input and output data for the copy operation.</span></span> <span data-ttu-id="489d9-124">V příkladu uvedených v posledním kroku vytvoříte datové sady a zadat kontejner objektů blob a složky, která obsahuje vstupní data.</span><span class="sxs-lookup"><span data-stu-id="489d9-124">In the example mentioned in the last step, you create a dataset to specify the blob container and folder that contains the input data.</span></span> <span data-ttu-id="489d9-125">A vytvořte jinou datovou sadu, která zadejte tabulku SQL ve službě Azure SQL database, který obsahuje data zkopírovat z úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="489d9-125">And, you create another dataset to specify the SQL table in the Azure SQL database  that holds the data copied from the blob storage.</span></span> <span data-ttu-id="489d9-126">Vlastnosti datové sady, které jsou specifické pro Azure Data Lake Store, naleznete v části [vlastnosti datové sady](#dataset-properties) části.</span><span class="sxs-lookup"><span data-stu-id="489d9-126">For dataset properties that are specific to Azure Data Lake Store, see [dataset properties](#dataset-properties) section.</span></span>
4. <span data-ttu-id="489d9-127">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="489d9-127">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="489d9-128">V příkladu již bylo zmíněno dříve použijete BlobSource jako zdroj a SqlSink jako jímku pro aktivitu kopírování.</span><span class="sxs-lookup"><span data-stu-id="489d9-128">In the example mentioned earlier, you use BlobSource as a source and SqlSink as a sink for the copy activity.</span></span> <span data-ttu-id="489d9-129">Podobně pokud kopírujete z databáze SQL Azure do Azure Blob Storage, můžete použít SqlSource a BlobSink v aktivitě kopírování.</span><span class="sxs-lookup"><span data-stu-id="489d9-129">Similarly, if you are copying from Azure SQL Database to Azure Blob Storage, you use SqlSource and BlobSink in the copy activity.</span></span> <span data-ttu-id="489d9-130">Kopírovat vlastnosti aktivity, které jsou specifické pro Azure SQL Database, najdete v části [zkopírovat vlastnosti aktivity](#copy-activity-properties) části.</span><span class="sxs-lookup"><span data-stu-id="489d9-130">For copy activity properties that are specific to Azure SQL Database, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="489d9-131">Podrobnosti o tom, jak používat úložiště dat jako zdroj nebo jímka klikněte na odkaz v předchozí části pro data store.</span><span class="sxs-lookup"><span data-stu-id="489d9-131">For details on how to use a data store as a source or a sink, click the link in the previous section for your data store.</span></span>

<span data-ttu-id="489d9-132">Když použijete průvodce, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál).</span><span class="sxs-lookup"><span data-stu-id="489d9-132">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="489d9-133">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), definujete tyto entity služby Data Factory pomocí formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="489d9-133">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="489d9-134">Ukázky s definicemi JSON entit služby Data Factory, které se používají ke zkopírování dat do/z Azure SQL Database, najdete v části [JSON příklady](#json-examples-for-copying-data-to-and-from-sql-database) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="489d9-134">For samples with JSON definitions for Data Factory entities that are used to copy data to/from an Azure SQL Database, see [JSON examples](#json-examples-for-copying-data-to-and-from-sql-database) section of this article.</span></span> 

<span data-ttu-id="489d9-135">Následující části obsahují podrobnosti o vlastnostech formátu JSON, které slouží k určení entit služby Data Factory konkrétní do Azure SQL Database:</span><span class="sxs-lookup"><span data-stu-id="489d9-135">The following sections provide details about JSON properties that are used to define Data Factory entities specific to Azure SQL Database:</span></span> 

## <a name="linked-service-properties"></a><span data-ttu-id="489d9-136">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="489d9-136">Linked service properties</span></span>
<span data-ttu-id="489d9-137">Azure SQL propojená propojuje službu Azure SQL database pro vytváření dat..</span><span class="sxs-lookup"><span data-stu-id="489d9-137">An Azure SQL linked service links an Azure SQL database to your data factory.</span></span> <span data-ttu-id="489d9-138">Následující tabulka obsahuje popis JSON elementy, které jsou specifické pro propojené služby Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="489d9-138">The following table provides description for JSON elements specific to Azure SQL linked service.</span></span>

| <span data-ttu-id="489d9-139">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="489d9-139">Property</span></span> | <span data-ttu-id="489d9-140">Popis</span><span class="sxs-lookup"><span data-stu-id="489d9-140">Description</span></span> | <span data-ttu-id="489d9-141">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="489d9-141">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="489d9-142">type</span><span class="sxs-lookup"><span data-stu-id="489d9-142">type</span></span> |<span data-ttu-id="489d9-143">Vlastnost typu musí být nastavena na: **azuresqldatabase.**</span><span class="sxs-lookup"><span data-stu-id="489d9-143">The type property must be set to: **AzureSqlDatabase**</span></span> |<span data-ttu-id="489d9-144">Ano</span><span class="sxs-lookup"><span data-stu-id="489d9-144">Yes</span></span> |
| <span data-ttu-id="489d9-145">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="489d9-145">connectionString</span></span> |<span data-ttu-id="489d9-146">Zadejte informace potřebné pro připojení k instanci databáze SQL Azure pro vlastnost connectionString.</span><span class="sxs-lookup"><span data-stu-id="489d9-146">Specify information needed to connect to the Azure SQL Database instance for the connectionString property.</span></span> <span data-ttu-id="489d9-147">Podporováno je pouze základní ověřování.</span><span class="sxs-lookup"><span data-stu-id="489d9-147">Only basic authentication is supported.</span></span> |<span data-ttu-id="489d9-148">Ano</span><span class="sxs-lookup"><span data-stu-id="489d9-148">Yes</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="489d9-149">Konfigurace [Firewall databáze Azure SQL](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) databázový server, který [povolit službám Azure přístup k serveru](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure).</span><span class="sxs-lookup"><span data-stu-id="489d9-149">Configure [Azure SQL Database Firewall](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) the database server to [allow Azure Services to access the server](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure).</span></span> <span data-ttu-id="489d9-150">Kromě toho pokud data kopírujete do Azure SQL Database z mimo Azure včetně z místního zdroje dat se brána objekt pro vytváření dat, nakonfigurujte příslušné rozsah IP adres pro počítač, který odesílá data do Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="489d9-150">Additionally, if you are copying data to Azure SQL Database from outside Azure including from on-premises data sources with data factory gateway, configure appropriate IP address range for the machine that is sending data to Azure SQL Database.</span></span>

## <a name="dataset-properties"></a><span data-ttu-id="489d9-151">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="489d9-151">Dataset properties</span></span>
<span data-ttu-id="489d9-152">Pokud chcete zadat datové sady představují vstupních nebo výstupních dat v Azure SQL database, nastavíte vlastnost type datové sady, která: **AzureSqlTable**.</span><span class="sxs-lookup"><span data-stu-id="489d9-152">To specify a dataset to represent input or output data in an Azure SQL database, you set the type property of the dataset to: **AzureSqlTable**.</span></span> <span data-ttu-id="489d9-153">Nastavte **linkedServiceName** vlastnosti datové sady, která název Azure SQL, propojené služby.</span><span class="sxs-lookup"><span data-stu-id="489d9-153">Set the **linkedServiceName** property of the dataset to the name of the Azure SQL linked service.</span></span>  

<span data-ttu-id="489d9-154">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v článku [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="489d9-154">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="489d9-155">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="489d9-155">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="489d9-156">V rámci typeProperties části se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění dat v úložišti.</span><span class="sxs-lookup"><span data-stu-id="489d9-156">The typeProperties section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="489d9-157">**Rámci typeProperties** části datové sady typu **AzureSqlTable** má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="489d9-157">The **typeProperties** section for the dataset of type **AzureSqlTable** has the following properties:</span></span>

| <span data-ttu-id="489d9-158">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="489d9-158">Property</span></span> | <span data-ttu-id="489d9-159">Popis</span><span class="sxs-lookup"><span data-stu-id="489d9-159">Description</span></span> | <span data-ttu-id="489d9-160">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="489d9-160">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="489d9-161">tableName</span><span class="sxs-lookup"><span data-stu-id="489d9-161">tableName</span></span> |<span data-ttu-id="489d9-162">Název tabulky nebo zobrazení instance databáze SQL Azure, kterou propojená služba odkazuje.</span><span class="sxs-lookup"><span data-stu-id="489d9-162">Name of the table or view in the Azure SQL Database instance that linked service refers to.</span></span> |<span data-ttu-id="489d9-163">Ano</span><span class="sxs-lookup"><span data-stu-id="489d9-163">Yes</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="489d9-164">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="489d9-164">Copy activity properties</span></span>
<span data-ttu-id="489d9-165">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="489d9-165">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="489d9-166">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="489d9-166">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

> [!NOTE]
> <span data-ttu-id="489d9-167">Aktivita kopírování přijímá pouze jeden vstup a vytváří jenom jeden výstup.</span><span class="sxs-lookup"><span data-stu-id="489d9-167">The Copy Activity takes only one input and produces only one output.</span></span>

<span data-ttu-id="489d9-168">Vzhledem k tomu, vlastnosti dostupné ve **rámci typeProperties** části aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="489d9-168">Whereas, properties available in the **typeProperties** section of the activity vary with each activity type.</span></span> <span data-ttu-id="489d9-169">Pro aktivitu kopírování budou lišit v závislosti na typech zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="489d9-169">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="489d9-170">Pokud přesouváte data z databáze Azure SQL, nastavíte typ zdroje v aktivitě kopírování do **SqlSource**.</span><span class="sxs-lookup"><span data-stu-id="489d9-170">If you are moving data from an Azure SQL database, you set the source type in the copy activity to **SqlSource**.</span></span> <span data-ttu-id="489d9-171">Podobně pokud přesouváte data do Azure SQL database, nastavíte typ jímky v aktivitě kopírování do **SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="489d9-171">Similarly, if you are moving data to an Azure SQL database, you set the sink type in the copy activity to **SqlSink**.</span></span> <span data-ttu-id="489d9-172">Tato část obsahuje seznam vlastností, které jsou podporované SqlSource a SqlSink.</span><span class="sxs-lookup"><span data-stu-id="489d9-172">This section provides a list of properties supported by SqlSource and SqlSink.</span></span>

### <a name="sqlsource"></a><span data-ttu-id="489d9-173">SqlSource</span><span class="sxs-lookup"><span data-stu-id="489d9-173">SqlSource</span></span>
<span data-ttu-id="489d9-174">Při aktivitě kopírování, pokud je zdroj typu **SqlSource**, následující vlastnosti jsou k dispozici v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="489d9-174">In copy activity, when the source is of type **SqlSource**, the following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="489d9-175">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="489d9-175">Property</span></span> | <span data-ttu-id="489d9-176">Popis</span><span class="sxs-lookup"><span data-stu-id="489d9-176">Description</span></span> | <span data-ttu-id="489d9-177">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="489d9-177">Allowed values</span></span> | <span data-ttu-id="489d9-178">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="489d9-178">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="489d9-179">sqlReaderQuery</span><span class="sxs-lookup"><span data-stu-id="489d9-179">sqlReaderQuery</span></span> |<span data-ttu-id="489d9-180">Čtení dat pomocí vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="489d9-180">Use the custom query to read data.</span></span> |<span data-ttu-id="489d9-181">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="489d9-181">SQL query string.</span></span> <span data-ttu-id="489d9-182">Příklad: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="489d9-182">Example: `select * from MyTable`.</span></span> |<span data-ttu-id="489d9-183">Ne</span><span class="sxs-lookup"><span data-stu-id="489d9-183">No</span></span> |
| <span data-ttu-id="489d9-184">sqlReaderStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="489d9-184">sqlReaderStoredProcedureName</span></span> |<span data-ttu-id="489d9-185">Název uložené procedury, který čte data ze zdrojové tabulky.</span><span class="sxs-lookup"><span data-stu-id="489d9-185">Name of the stored procedure that reads data from the source table.</span></span> |<span data-ttu-id="489d9-186">Název uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="489d9-186">Name of the stored procedure.</span></span> <span data-ttu-id="489d9-187">Poslední příkaz jazyka SQL musí být příkaz SELECT v uložené proceduře.</span><span class="sxs-lookup"><span data-stu-id="489d9-187">The last SQL statement must be a SELECT statement in the stored procedure.</span></span> |<span data-ttu-id="489d9-188">Ne</span><span class="sxs-lookup"><span data-stu-id="489d9-188">No</span></span> |
| <span data-ttu-id="489d9-189">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="489d9-189">storedProcedureParameters</span></span> |<span data-ttu-id="489d9-190">Parametry pro uloženou proceduru.</span><span class="sxs-lookup"><span data-stu-id="489d9-190">Parameters for the stored procedure.</span></span> |<span data-ttu-id="489d9-191">Páry název/hodnota.</span><span class="sxs-lookup"><span data-stu-id="489d9-191">Name/value pairs.</span></span> <span data-ttu-id="489d9-192">Názvy a malá a velká písmena parametry musí odpovídat názvům a malá a velká písmena parametry uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="489d9-192">Names and casing of parameters must match the names and casing of the stored procedure parameters.</span></span> |<span data-ttu-id="489d9-193">Ne</span><span class="sxs-lookup"><span data-stu-id="489d9-193">No</span></span> |

<span data-ttu-id="489d9-194">Pokud **sqlReaderQuery** je zadán pro SqlSource, aktivitě kopírování spouští tento dotaz zdrojů databáze SQL Azure a získat data.</span><span class="sxs-lookup"><span data-stu-id="489d9-194">If the **sqlReaderQuery** is specified for the SqlSource, the Copy Activity runs this query against the Azure SQL Database source to get the data.</span></span> <span data-ttu-id="489d9-195">Alternativně můžete zadat uložené procedury zadáním **sqlReaderStoredProcedureName** a **storedProcedureParameters** (Pokud uložená procedura přebírá parametry).</span><span class="sxs-lookup"><span data-stu-id="489d9-195">Alternatively, you can specify a stored procedure by specifying the **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if the stored procedure takes parameters).</span></span>

<span data-ttu-id="489d9-196">Pokud nezadáte sqlReaderQuery nebo sqlReaderStoredProcedureName, sloupce definované v části Struktura sady dat JSON se používají k vytvoření dotazu (`select column1, column2 from mytable`) ke spouštění na databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="489d9-196">If you do not specify either sqlReaderQuery or sqlReaderStoredProcedureName, the columns defined in the structure section of the dataset JSON are used to build a query (`select column1, column2 from mytable`) to run against the Azure SQL Database.</span></span> <span data-ttu-id="489d9-197">Pokud definice datové sady nemá strukturu, jsou vybrány všechny sloupce z tabulky.</span><span class="sxs-lookup"><span data-stu-id="489d9-197">If the dataset definition does not have the structure, all columns are selected from the table.</span></span>

> [!NOTE]
> <span data-ttu-id="489d9-198">Při použití **sqlReaderStoredProcedureName**, stále je třeba zadat hodnotu pro **tableName** vlastnost v datové sadě JSON.</span><span class="sxs-lookup"><span data-stu-id="489d9-198">When you use **sqlReaderStoredProcedureName**, you still need to specify a value for the **tableName** property in the dataset JSON.</span></span> <span data-ttu-id="489d9-199">Neexistují žádné ověření, ale adresovat této tabulky.</span><span class="sxs-lookup"><span data-stu-id="489d9-199">There are no validations performed against this table though.</span></span>
>
>

### <a name="sqlsource-example"></a><span data-ttu-id="489d9-200">Příklad SqlSource</span><span class="sxs-lookup"><span data-stu-id="489d9-200">SqlSource example</span></span>

```JSON
"source": {
    "type": "SqlSource",
    "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
    "storedProcedureParameters": {
        "stringData": { "value": "str3" },
        "identifier": { "value": "$$Text.Format('{0:yyyy}', SliceStart)", "type": "Int"}
    }
}
```

<span data-ttu-id="489d9-201">**Uložená procedura definice:**</span><span class="sxs-lookup"><span data-stu-id="489d9-201">**The stored procedure definition:**</span></span>

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

### <a name="sqlsink"></a><span data-ttu-id="489d9-202">SqlSink</span><span class="sxs-lookup"><span data-stu-id="489d9-202">SqlSink</span></span>
<span data-ttu-id="489d9-203">**SqlSink** podporuje následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="489d9-203">**SqlSink** supports the following properties:</span></span>

| <span data-ttu-id="489d9-204">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="489d9-204">Property</span></span> | <span data-ttu-id="489d9-205">Popis</span><span class="sxs-lookup"><span data-stu-id="489d9-205">Description</span></span> | <span data-ttu-id="489d9-206">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="489d9-206">Allowed values</span></span> | <span data-ttu-id="489d9-207">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="489d9-207">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="489d9-208">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="489d9-208">writeBatchTimeout</span></span> |<span data-ttu-id="489d9-209">Počkejte, než čas na dokončení předtím, než vyprší časový limit operace dávkové vložení.</span><span class="sxs-lookup"><span data-stu-id="489d9-209">Wait time for the batch insert operation to complete before it times out.</span></span> |<span data-ttu-id="489d9-210">Časový interval</span><span class="sxs-lookup"><span data-stu-id="489d9-210">timespan</span></span><br/><br/> <span data-ttu-id="489d9-211">Příklad: "00: 30:00" (30 minut).</span><span class="sxs-lookup"><span data-stu-id="489d9-211">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="489d9-212">Ne</span><span class="sxs-lookup"><span data-stu-id="489d9-212">No</span></span> |
| <span data-ttu-id="489d9-213">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="489d9-213">writeBatchSize</span></span> |<span data-ttu-id="489d9-214">Vloží data do tabulky SQL, když velikost vyrovnávací paměti dosáhne writeBatchSize.</span><span class="sxs-lookup"><span data-stu-id="489d9-214">Inserts data into the SQL table when the buffer size reaches writeBatchSize.</span></span> |<span data-ttu-id="489d9-215">Celé číslo (počet řádků)</span><span class="sxs-lookup"><span data-stu-id="489d9-215">Integer (number of rows)</span></span> |<span data-ttu-id="489d9-216">Ne (výchozí: 10000)</span><span class="sxs-lookup"><span data-stu-id="489d9-216">No (default: 10000)</span></span> |
| <span data-ttu-id="489d9-217">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="489d9-217">sqlWriterCleanupScript</span></span> |<span data-ttu-id="489d9-218">Zadejte dotaz pro aktivitu kopírování provést tak, aby se vyčistit data určitý řez.</span><span class="sxs-lookup"><span data-stu-id="489d9-218">Specify a query for Copy Activity to execute such that data of a specific slice is cleaned up.</span></span> <span data-ttu-id="489d9-219">Další informace najdete v tématu [opakovatelných kopie](#repeatable-copy).</span><span class="sxs-lookup"><span data-stu-id="489d9-219">For more information, see [repeatable copy](#repeatable-copy).</span></span> |<span data-ttu-id="489d9-220">Příkaz dotazu.</span><span class="sxs-lookup"><span data-stu-id="489d9-220">A query statement.</span></span> |<span data-ttu-id="489d9-221">Ne</span><span class="sxs-lookup"><span data-stu-id="489d9-221">No</span></span> |
| <span data-ttu-id="489d9-222">sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="489d9-222">sliceIdentifierColumnName</span></span> |<span data-ttu-id="489d9-223">Zadejte název sloupce pro aktivitu kopírování vyplníte identifikátor automaticky generovány řez, který se používá k vyčištění dat určitý řez při spusťte znovu.</span><span class="sxs-lookup"><span data-stu-id="489d9-223">Specify a column name for Copy Activity to fill with auto generated slice identifier, which is used to clean up data of a specific slice when rerun.</span></span> <span data-ttu-id="489d9-224">Další informace najdete v tématu [opakovatelných kopie](#repeatable-copy).</span><span class="sxs-lookup"><span data-stu-id="489d9-224">For more information, see [repeatable copy](#repeatable-copy).</span></span> |<span data-ttu-id="489d9-225">Název sloupce sloupce s datovým typem binary(32).</span><span class="sxs-lookup"><span data-stu-id="489d9-225">Column name of a column with data type of binary(32).</span></span> |<span data-ttu-id="489d9-226">Ne</span><span class="sxs-lookup"><span data-stu-id="489d9-226">No</span></span> |
| <span data-ttu-id="489d9-227">sqlWriterStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="489d9-227">sqlWriterStoredProcedureName</span></span> |<span data-ttu-id="489d9-228">Název uložené procedury upserts (aktualizace nebo vložení) dat do cílové tabulky.</span><span class="sxs-lookup"><span data-stu-id="489d9-228">Name of the stored procedure that upserts (updates/inserts) data into the target table.</span></span> |<span data-ttu-id="489d9-229">Název uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="489d9-229">Name of the stored procedure.</span></span> |<span data-ttu-id="489d9-230">Ne</span><span class="sxs-lookup"><span data-stu-id="489d9-230">No</span></span> |
| <span data-ttu-id="489d9-231">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="489d9-231">storedProcedureParameters</span></span> |<span data-ttu-id="489d9-232">Parametry pro uloženou proceduru.</span><span class="sxs-lookup"><span data-stu-id="489d9-232">Parameters for the stored procedure.</span></span> |<span data-ttu-id="489d9-233">Páry název/hodnota.</span><span class="sxs-lookup"><span data-stu-id="489d9-233">Name/value pairs.</span></span> <span data-ttu-id="489d9-234">Názvy a malá a velká písmena parametry musí odpovídat názvům a malá a velká písmena parametry uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="489d9-234">Names and casing of parameters must match the names and casing of the stored procedure parameters.</span></span> |<span data-ttu-id="489d9-235">Ne</span><span class="sxs-lookup"><span data-stu-id="489d9-235">No</span></span> |
| <span data-ttu-id="489d9-236">sqlWriterTableType</span><span class="sxs-lookup"><span data-stu-id="489d9-236">sqlWriterTableType</span></span> |<span data-ttu-id="489d9-237">Zadejte název typu tabulky má být použit v uložené proceduře.</span><span class="sxs-lookup"><span data-stu-id="489d9-237">Specify a table type name to be used in the stored procedure.</span></span> <span data-ttu-id="489d9-238">Aktivita kopírování zpřístupní přesouvání dat v dočasné tabulce s tímto typem tabulky.</span><span class="sxs-lookup"><span data-stu-id="489d9-238">Copy activity makes the data being moved available in a temp table with this table type.</span></span> <span data-ttu-id="489d9-239">Uložená procedura kód pak sloučit data kopírovány s existujícími daty.</span><span class="sxs-lookup"><span data-stu-id="489d9-239">Stored procedure code can then merge the data being copied with existing data.</span></span> |<span data-ttu-id="489d9-240">Zadejte název tabulky.</span><span class="sxs-lookup"><span data-stu-id="489d9-240">A table type name.</span></span> |<span data-ttu-id="489d9-241">Ne</span><span class="sxs-lookup"><span data-stu-id="489d9-241">No</span></span> |

#### <a name="sqlsink-example"></a><span data-ttu-id="489d9-242">Příklad SqlSink</span><span class="sxs-lookup"><span data-stu-id="489d9-242">SqlSink example</span></span>

```JSON
"sink": {
    "type": "SqlSink",
    "writeBatchSize": 1000000,
    "writeBatchTimeout": "00:05:00",
    "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
    "sqlWriterTableType": "CopyTestTableType",
    "storedProcedureParameters": {
        "identifier": { "value": "1", "type": "Int" },
        "stringData": { "value": "str1" },
        "decimalData": { "value": "1", "type": "Decimal" }
    }
}
```

## <a name="json-examples-for-copying-data-to-and-from-sql-database"></a><span data-ttu-id="489d9-243">Příklady JSON pro kopírování dat do a z databáze SQL</span><span class="sxs-lookup"><span data-stu-id="489d9-243">JSON examples for copying data to and from SQL Database</span></span>
<span data-ttu-id="489d9-244">Následující příklady poskytují ukázka JSON definice, které můžete použít k vytvoření kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="489d9-244">The following examples provide sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="489d9-245">Se ukazují, jak ke zkopírování dat do a z Azure SQL Database a Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="489d9-245">They show how to copy data to and from Azure SQL Database and Azure Blob Storage.</span></span> <span data-ttu-id="489d9-246">Nicméně je možné zkopírovat data **přímo** ze všech zdrojů do jakéhokoli z jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí aktivity kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="489d9-246">However, data can be copied **directly** from any of sources to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>

### <a name="example-copy-data-from-azure-sql-database-to-azure-blob"></a><span data-ttu-id="489d9-247">Příklad: Kopírování dat z databáze SQL Azure do Azure Blob</span><span class="sxs-lookup"><span data-stu-id="489d9-247">Example: Copy data from Azure SQL Database to Azure Blob</span></span>
<span data-ttu-id="489d9-248">Stejné definuje následující entity služby Data Factory:</span><span class="sxs-lookup"><span data-stu-id="489d9-248">The same defines the following Data Factory entities:</span></span>

1. <span data-ttu-id="489d9-249">Propojené služby typu [azuresqldatabase](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="489d9-249">A linked service of type [AzureSqlDatabase](#linked-service-properties).</span></span>
2. <span data-ttu-id="489d9-250">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="489d9-250">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="489d9-251">Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureSqlTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="489d9-251">An input [dataset](data-factory-create-datasets.md) of type [AzureSqlTable](#dataset-properties).</span></span>
4. <span data-ttu-id="489d9-252">Výstup [datovou sadu](data-factory-create-datasets.md) typu [objektů Blob v Azure](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="489d9-252">An output [dataset](data-factory-create-datasets.md) of type [Azure Blob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="489d9-253">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [SqlSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="489d9-253">A [pipeline](data-factory-create-pipelines.md) with a Copy activity that uses [SqlSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="489d9-254">Ukázka zkopíruje data časové řady (hodinový, denní, atd.) z tabulky v databázi Azure SQL do objektu blob každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="489d9-254">The sample copies time-series data (hourly, daily, etc.) from a table in Azure SQL database to a blob every hour.</span></span> <span data-ttu-id="489d9-255">Vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="489d9-255">The JSON properties used in these samples are described in sections following the samples.</span></span>  

<span data-ttu-id="489d9-256">**Azure SQL Database propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="489d9-256">**Azure SQL Database linked service:**</span></span>

```JSON
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
<span data-ttu-id="489d9-257">Najdete v článku [propojená služba Azure SQL](#linked-service) části seznamu vlastností podporuje tuto propojenou službu.</span><span class="sxs-lookup"><span data-stu-id="489d9-257">See the [Azure SQL Linked Service](#linked-service) section for the list of properties supported by this linked service.</span></span>

<span data-ttu-id="489d9-258">**Propojená služba Azure Blob storage:**</span><span class="sxs-lookup"><span data-stu-id="489d9-258">**Azure Blob storage linked service:**</span></span>

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
<span data-ttu-id="489d9-259">Najdete v článku [objektů Blob v Azure](data-factory-azure-blob-connector.md#azure-storage-linked-service) článku seznamu vlastností podporuje tuto propojenou službu.</span><span class="sxs-lookup"><span data-stu-id="489d9-259">See the [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) article for the list of properties supported by this linked service.</span></span>


<span data-ttu-id="489d9-260">**Azure SQL vstupní datové sady:**</span><span class="sxs-lookup"><span data-stu-id="489d9-260">**Azure SQL input dataset:**</span></span>

<span data-ttu-id="489d9-261">Příkladu se předpokládá, jste vytvořili tabulku "MyTable" v Azure SQL a obsahuje sloupec s názvem "timestampcolumn" pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="489d9-261">The sample assumes you have created a table “MyTable” in Azure SQL and it contains a column called “timestampcolumn” for time series data.</span></span>

<span data-ttu-id="489d9-262">Nastavení "externí": "PRAVDA" informuje služby Azure Data Factory, datová sada je externí k objektu pro vytváření dat a není vyprodukované aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="489d9-262">Setting “external”: ”true” informs the Azure Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

```JSON
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

<span data-ttu-id="489d9-263">Najdete v článku [vlastnosti typu datové sady Azure SQL](#dataset) části seznamu vlastností nepodporuje tento typ dataset.</span><span class="sxs-lookup"><span data-stu-id="489d9-263">See the [Azure SQL dataset type properties](#dataset) section for the list of properties supported by this dataset type.</span></span>  

<span data-ttu-id="489d9-264">**Azure Blob výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="489d9-264">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="489d9-265">Data se zapisují do nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="489d9-265">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="489d9-266">Cesta ke složce pro tento objekt blob je vyhodnocován dynamicky podle času zahájení řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="489d9-266">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="489d9-267">Cesta ke složce používá rok, měsíc, den a čas částí čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="489d9-267">The folder path uses year, month, day, and hours parts of the start time.</span></span>

```JSON
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
<span data-ttu-id="489d9-268">Najdete v článku [vlastnosti typu datovou sadu objektu Blob Azure](data-factory-azure-blob-connector.md#dataset-properties) části seznamu vlastností nepodporuje tento typ dataset.</span><span class="sxs-lookup"><span data-stu-id="489d9-268">See the [Azure Blob dataset type properties](data-factory-azure-blob-connector.md#dataset-properties) section for the list of properties supported by this dataset type.</span></span>  

<span data-ttu-id="489d9-269">**Aktivita kopírování v kanálu s SQL zdrojový a podřízený objekt Blob:**</span><span class="sxs-lookup"><span data-stu-id="489d9-269">**A copy activity in a pipeline with SQL source and Blob sink:**</span></span>

<span data-ttu-id="489d9-270">Kanál obsahuje aktivitu kopírování, který je nakonfigurovaný na použití vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="489d9-270">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="489d9-271">V definici JSON kanálu **zdroj** je typ nastaven na **SqlSource** a **podřízený** je typ nastaven na **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="489d9-271">In the pipeline JSON definition, the **source** type is set to **SqlSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="489d9-272">Zadané pro dotaz SQL **SqlReaderQuery** vlastnost vybere data za poslední hodinu pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="489d9-272">The SQL query specified for the **SqlReaderQuery** property selects the data in the past hour to copy.</span></span>

```JSON
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
<span data-ttu-id="489d9-273">V příkladu **sqlReaderQuery** je zadán pro SqlSource.</span><span class="sxs-lookup"><span data-stu-id="489d9-273">In the example, **sqlReaderQuery** is specified for the SqlSource.</span></span> <span data-ttu-id="489d9-274">Aktivita kopírování spouští tento dotaz zdrojů databáze SQL Azure a získat data.</span><span class="sxs-lookup"><span data-stu-id="489d9-274">The Copy Activity runs this query against the Azure SQL Database source to get the data.</span></span> <span data-ttu-id="489d9-275">Alternativně můžete zadat uložené procedury zadáním **sqlReaderStoredProcedureName** a **storedProcedureParameters** (Pokud uložená procedura přebírá parametry).</span><span class="sxs-lookup"><span data-stu-id="489d9-275">Alternatively, you can specify a stored procedure by specifying the **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if the stored procedure takes parameters).</span></span>

<span data-ttu-id="489d9-276">Pokud nezadáte sqlReaderQuery nebo sqlReaderStoredProcedureName, sloupce definované v části Struktura sady dat JSON se používají k vytvoření dotazu ke spouštění na databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="489d9-276">If you do not specify either sqlReaderQuery or sqlReaderStoredProcedureName, the columns defined in the structure section of the dataset JSON are used to build a query to run against the Azure SQL Database.</span></span> <span data-ttu-id="489d9-277">Například: `select column1, column2 from mytable`.</span><span class="sxs-lookup"><span data-stu-id="489d9-277">For example: `select column1, column2 from mytable`.</span></span> <span data-ttu-id="489d9-278">Pokud definice datové sady nemá strukturu, jsou vybrány všechny sloupce z tabulky.</span><span class="sxs-lookup"><span data-stu-id="489d9-278">If the dataset definition does not have the structure, all columns are selected from the table.</span></span>

<span data-ttu-id="489d9-279">Najdete v článku [zdroje Sql](#sqlsource) části a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) pro seznam vlastností, které jsou podporované SqlSource a BlobSink.</span><span class="sxs-lookup"><span data-stu-id="489d9-279">See the [Sql Source](#sqlsource) section and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) for the list of properties supported by SqlSource and BlobSink.</span></span>

### <a name="example-copy-data-from-azure-blob-to-azure-sql-database"></a><span data-ttu-id="489d9-280">Příklad: Kopírování dat z objektu Blob Azure do Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="489d9-280">Example: Copy data from Azure Blob to Azure SQL Database</span></span>
<span data-ttu-id="489d9-281">Ukázka definuje následující entity služby Data Factory:</span><span class="sxs-lookup"><span data-stu-id="489d9-281">The sample defines the following Data Factory entities:</span></span>  

1. <span data-ttu-id="489d9-282">Propojené služby typu [azuresqldatabase](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="489d9-282">A linked service of type [AzureSqlDatabase](#linked-service-properties).</span></span>
2. <span data-ttu-id="489d9-283">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="489d9-283">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="489d9-284">Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="489d9-284">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="489d9-285">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureSqlTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="489d9-285">An output [dataset](data-factory-create-datasets.md) of type [AzureSqlTable](#dataset-properties).</span></span>
5. <span data-ttu-id="489d9-286">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) a [SqlSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="489d9-286">A [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) and [SqlSink](#copy-activity-properties).</span></span>

<span data-ttu-id="489d9-287">Kopie ukázka časové řady dat (hodinový, denní, atd.) z Azure blob do tabulky v Azure SQL databáze každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="489d9-287">The sample copies time-series data (hourly, daily, etc.) from Azure blob to a table in Azure SQL database every hour.</span></span> <span data-ttu-id="489d9-288">Vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="489d9-288">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="489d9-289">**Azure SQL propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="489d9-289">**Azure SQL linked service:**</span></span>

```JSON
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
<span data-ttu-id="489d9-290">Najdete v článku [propojená služba Azure SQL](#linked-service) části seznamu vlastností podporuje tuto propojenou službu.</span><span class="sxs-lookup"><span data-stu-id="489d9-290">See the [Azure SQL Linked Service](#linked-service) section for the list of properties supported by this linked service.</span></span>

<span data-ttu-id="489d9-291">**Propojená služba Azure Blob storage:**</span><span class="sxs-lookup"><span data-stu-id="489d9-291">**Azure Blob storage linked service:**</span></span>

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
<span data-ttu-id="489d9-292">Najdete v článku [objektů Blob v Azure](data-factory-azure-blob-connector.md#azure-storage-linked-service) článku seznamu vlastností podporuje tuto propojenou službu.</span><span class="sxs-lookup"><span data-stu-id="489d9-292">See the [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) article for the list of properties supported by this linked service.</span></span>


<span data-ttu-id="489d9-293">**Azure vstupní datovou sadu objektu Blob:**</span><span class="sxs-lookup"><span data-stu-id="489d9-293">**Azure Blob input dataset:**</span></span>

<span data-ttu-id="489d9-294">Data je převzata z nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="489d9-294">Data is picked up from a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="489d9-295">Název složky a cesta k souboru pro tento objekt blob se vyhodnocují dynamicky podle času zahájení řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="489d9-295">The folder path and file name for the blob are dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="489d9-296">Cesta ke složce používá rok, měsíc a den součástí čas spuštění a název souboru používá hodinu součástí čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="489d9-296">The folder path uses year, month, and day part of the start time and file name uses the hour part of the start time.</span></span> <span data-ttu-id="489d9-297">"externí": "PRAVDA" nastavení informuje služba Data Factory, že tato tabulka je externí k objektu pro vytváření dat a není vyprodukované aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="489d9-297">“external”: “true” setting informs the Data Factory service that this table is external to the data factory and is not produced by an activity in the data factory.</span></span>

```JSON
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/",
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
<span data-ttu-id="489d9-298">Najdete v článku [vlastnosti typu datovou sadu objektu Blob Azure](data-factory-azure-blob-connector.md#dataset-properties) části seznamu vlastností nepodporuje tento typ dataset.</span><span class="sxs-lookup"><span data-stu-id="489d9-298">See the [Azure Blob dataset type properties](data-factory-azure-blob-connector.md#dataset-properties) section for the list of properties supported by this dataset type.</span></span>

<span data-ttu-id="489d9-299">**Azure SQL Database výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="489d9-299">**Azure SQL Database output dataset:**</span></span>

<span data-ttu-id="489d9-300">Ukázka zkopíruje data na tabulku s názvem "MyTable" v Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="489d9-300">The sample copies data to a table named “MyTable” in Azure SQL.</span></span> <span data-ttu-id="489d9-301">Vytvořte v tabulce v Azure SQL s stejný počet sloupců, podle očekávání souboru CSV objektů Blob tak, aby obsahovala.</span><span class="sxs-lookup"><span data-stu-id="489d9-301">Create the table in Azure SQL with the same number of columns as you expect the Blob CSV file to contain.</span></span> <span data-ttu-id="489d9-302">Nové záznamy se přidají do tabulky každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="489d9-302">New rows are added to the table every hour.</span></span>

```JSON
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
<span data-ttu-id="489d9-303">Najdete v článku [vlastnosti typu datové sady Azure SQL](#dataset) části seznamu vlastností nepodporuje tento typ dataset.</span><span class="sxs-lookup"><span data-stu-id="489d9-303">See the [Azure SQL dataset type properties](#dataset) section for the list of properties supported by this dataset type.</span></span>

<span data-ttu-id="489d9-304">**Aktivita kopírování v kanálu s Blob zdroj a jímka SQL:**</span><span class="sxs-lookup"><span data-stu-id="489d9-304">**A copy activity in a pipeline with Blob source and SQL sink:**</span></span>

<span data-ttu-id="489d9-305">Kanál obsahuje aktivitu kopírování, který je nakonfigurovaný na použití vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="489d9-305">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="489d9-306">V definici JSON kanálu **zdroj** je typ nastaven na **BlobSource** a **podřízený** je typ nastaven na **SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="489d9-306">In the pipeline JSON definition, the **source** type is set to **BlobSource** and **sink** type is set to **SqlSink**.</span></span>

```JSON
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
            "type": "BlobSource",
            "blobColumnSeparators": ","
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
<span data-ttu-id="489d9-307">Najdete v článku [jímku Sql](#sqlsink) části a [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) pro seznam vlastností, které jsou podporované SqlSink a BlobSource.</span><span class="sxs-lookup"><span data-stu-id="489d9-307">See the [Sql Sink](#sqlsink) section and [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) for the list of properties supported by SqlSink and BlobSource.</span></span>

## <a name="identity-columns-in-the-target-database"></a><span data-ttu-id="489d9-308">Sloupce identity v cílové databázi</span><span class="sxs-lookup"><span data-stu-id="489d9-308">Identity columns in the target database</span></span>
<span data-ttu-id="489d9-309">Tato část poskytuje příklad pro kopírování dat ze zdrojové tabulky bez sloupec identity do cílové tabulky se sloupcem identity.</span><span class="sxs-lookup"><span data-stu-id="489d9-309">This section provides an example for copying data from a source table without an identity column to a destination table with an identity column.</span></span>

<span data-ttu-id="489d9-310">**Zdrojová tabulka:**</span><span class="sxs-lookup"><span data-stu-id="489d9-310">**Source table:**</span></span>

```SQL
create table dbo.SourceTbl
(
       name varchar(100),
       age int
)
```
<span data-ttu-id="489d9-311">**Cílové tabulky:**</span><span class="sxs-lookup"><span data-stu-id="489d9-311">**Destination table:**</span></span>

```SQL
create table dbo.TargetTbl
(
       identifier int identity(1,1),
       name varchar(100),
       age int
)
```
<span data-ttu-id="489d9-312">Všimněte si, že cílová tabulka obsahuje sloupec identity.</span><span class="sxs-lookup"><span data-stu-id="489d9-312">Notice that the target table has an identity column.</span></span>

<span data-ttu-id="489d9-313">**Definice JSON datové sady zdroje**</span><span class="sxs-lookup"><span data-stu-id="489d9-313">**Source dataset JSON definition**</span></span>

```JSON
{
    "name": "SampleSource",
    "properties": {
        "type": " SqlServerTable",
        "linkedServiceName": "TestIdentitySQL",
        "typeProperties": {
            "tableName": "SourceTbl"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```
<span data-ttu-id="489d9-314">**Cílový definici JSON datové sady**</span><span class="sxs-lookup"><span data-stu-id="489d9-314">**Destination dataset JSON definition**</span></span>

```JSON
{
    "name": "SampleTarget",
    "properties": {
        "structure": [
            { "name": "name" },
            { "name": "age" }
        ],
        "type": "AzureSqlTable",
        "linkedServiceName": "TestIdentitySQLSource",
        "typeProperties": {
            "tableName": "TargetTbl"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": false,
        "policy": {}
    }    
}
```

<span data-ttu-id="489d9-315">Všimněte si, že jako zdrojové a cílové tabulky jiné schéma (cíl má sloupec s identitou).</span><span class="sxs-lookup"><span data-stu-id="489d9-315">Notice that as your source and target table have different schema (target has an additional column with identity).</span></span> <span data-ttu-id="489d9-316">V tomto scénáři budete muset zadat **struktura** vlastnost v definici datové sady cíl, který neobsahuje sloupec identity.</span><span class="sxs-lookup"><span data-stu-id="489d9-316">In this scenario, you need to specify **structure** property in the target dataset definition, which doesn’t include the identity column.</span></span>

## <a name="invoke-stored-procedure-from-sql-sink"></a><span data-ttu-id="489d9-317">Volání uložené procedury jímku SQL</span><span class="sxs-lookup"><span data-stu-id="489d9-317">Invoke stored procedure from SQL sink</span></span>
<span data-ttu-id="489d9-318">Příklad volání uložené procedury z jímku SQL při aktivitě kopírování kanálu, naleznete v části [vyvolat uloženou proceduru SQL jímka v aktivitě kopírování](data-factory-invoke-stored-procedure-from-copy-activity.md) článku.</span><span class="sxs-lookup"><span data-stu-id="489d9-318">For an example of invoking a stored procedure from SQL sink in a copy activity of a pipeline, see [Invoke stored procedure for SQL sink in copy activity](data-factory-invoke-stored-procedure-from-copy-activity.md) article.</span></span> 

## <a name="type-mapping-for-azure-sql-database"></a><span data-ttu-id="489d9-319">Mapování typu pro databázi SQL Azure</span><span class="sxs-lookup"><span data-stu-id="489d9-319">Type mapping for Azure SQL Database</span></span>
<span data-ttu-id="489d9-320">Jak je uvedeno v [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů zdroje do jímky typů s následující postup krok 2:</span><span class="sxs-lookup"><span data-stu-id="489d9-320">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article Copy activity performs automatic type conversions from source types to sink types with the following 2-step approach:</span></span>

1. <span data-ttu-id="489d9-321">Převést na typ .NET typy nativní zdrojů</span><span class="sxs-lookup"><span data-stu-id="489d9-321">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="489d9-322">Převést na typ jímky nativní typ formátu .NET</span><span class="sxs-lookup"><span data-stu-id="489d9-322">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="489d9-323">Při přesunu dat do a z Azure SQL Database, se používají následující mapování z typu SQL na typ .NET a naopak.</span><span class="sxs-lookup"><span data-stu-id="489d9-323">When moving data to and from Azure SQL Database, the following mappings are used from SQL type to .NET type and vice versa.</span></span> <span data-ttu-id="489d9-324">Mapování je stejný jako mapování SQL Server datového typu pro technologii ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="489d9-324">The mapping is same as the SQL Server Data Type Mapping for ADO.NET.</span></span>

| <span data-ttu-id="489d9-325">Typ databázového stroje SQL Server</span><span class="sxs-lookup"><span data-stu-id="489d9-325">SQL Server Database Engine type</span></span> | <span data-ttu-id="489d9-326">Typ rozhraní .NET framework</span><span class="sxs-lookup"><span data-stu-id="489d9-326">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="489d9-327">bigint</span><span class="sxs-lookup"><span data-stu-id="489d9-327">bigint</span></span> |<span data-ttu-id="489d9-328">Int64</span><span class="sxs-lookup"><span data-stu-id="489d9-328">Int64</span></span> |
| <span data-ttu-id="489d9-329">Binární</span><span class="sxs-lookup"><span data-stu-id="489d9-329">binary</span></span> |<span data-ttu-id="489d9-330">Byte]</span><span class="sxs-lookup"><span data-stu-id="489d9-330">Byte[]</span></span> |
| <span data-ttu-id="489d9-331">Bit</span><span class="sxs-lookup"><span data-stu-id="489d9-331">bit</span></span> |<span data-ttu-id="489d9-332">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="489d9-332">Boolean</span></span> |
| <span data-ttu-id="489d9-333">Char</span><span class="sxs-lookup"><span data-stu-id="489d9-333">char</span></span> |<span data-ttu-id="489d9-334">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="489d9-334">String, Char[]</span></span> |
| <span data-ttu-id="489d9-335">Datum</span><span class="sxs-lookup"><span data-stu-id="489d9-335">date</span></span> |<span data-ttu-id="489d9-336">Data a času</span><span class="sxs-lookup"><span data-stu-id="489d9-336">DateTime</span></span> |
| <span data-ttu-id="489d9-337">Data a času</span><span class="sxs-lookup"><span data-stu-id="489d9-337">Datetime</span></span> |<span data-ttu-id="489d9-338">Data a času</span><span class="sxs-lookup"><span data-stu-id="489d9-338">DateTime</span></span> |
| <span data-ttu-id="489d9-339">datetime2</span><span class="sxs-lookup"><span data-stu-id="489d9-339">datetime2</span></span> |<span data-ttu-id="489d9-340">Data a času</span><span class="sxs-lookup"><span data-stu-id="489d9-340">DateTime</span></span> |
| <span data-ttu-id="489d9-341">Datový typ DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="489d9-341">Datetimeoffset</span></span> |<span data-ttu-id="489d9-342">Datový typ DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="489d9-342">DateTimeOffset</span></span> |
| <span data-ttu-id="489d9-343">Decimal</span><span class="sxs-lookup"><span data-stu-id="489d9-343">Decimal</span></span> |<span data-ttu-id="489d9-344">Decimal</span><span class="sxs-lookup"><span data-stu-id="489d9-344">Decimal</span></span> |
| <span data-ttu-id="489d9-345">Atribut FILESTREAM (varbinary(max))</span><span class="sxs-lookup"><span data-stu-id="489d9-345">FILESTREAM attribute (varbinary(max))</span></span> |<span data-ttu-id="489d9-346">Byte]</span><span class="sxs-lookup"><span data-stu-id="489d9-346">Byte[]</span></span> |
| <span data-ttu-id="489d9-347">Plovoucí desetinná čárka</span><span class="sxs-lookup"><span data-stu-id="489d9-347">Float</span></span> |<span data-ttu-id="489d9-348">Double</span><span class="sxs-lookup"><span data-stu-id="489d9-348">Double</span></span> |
| <span data-ttu-id="489d9-349">Bitové kopie</span><span class="sxs-lookup"><span data-stu-id="489d9-349">image</span></span> |<span data-ttu-id="489d9-350">Byte]</span><span class="sxs-lookup"><span data-stu-id="489d9-350">Byte[]</span></span> |
| <span data-ttu-id="489d9-351">celá čísla</span><span class="sxs-lookup"><span data-stu-id="489d9-351">int</span></span> |<span data-ttu-id="489d9-352">Int32</span><span class="sxs-lookup"><span data-stu-id="489d9-352">Int32</span></span> |
| <span data-ttu-id="489d9-353">peníze</span><span class="sxs-lookup"><span data-stu-id="489d9-353">money</span></span> |<span data-ttu-id="489d9-354">Decimal</span><span class="sxs-lookup"><span data-stu-id="489d9-354">Decimal</span></span> |
| <span data-ttu-id="489d9-355">nchar</span><span class="sxs-lookup"><span data-stu-id="489d9-355">nchar</span></span> |<span data-ttu-id="489d9-356">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="489d9-356">String, Char[]</span></span> |
| <span data-ttu-id="489d9-357">ntext</span><span class="sxs-lookup"><span data-stu-id="489d9-357">ntext</span></span> |<span data-ttu-id="489d9-358">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="489d9-358">String, Char[]</span></span> |
| <span data-ttu-id="489d9-359">číselné</span><span class="sxs-lookup"><span data-stu-id="489d9-359">numeric</span></span> |<span data-ttu-id="489d9-360">Decimal</span><span class="sxs-lookup"><span data-stu-id="489d9-360">Decimal</span></span> |
| <span data-ttu-id="489d9-361">nvarchar</span><span class="sxs-lookup"><span data-stu-id="489d9-361">nvarchar</span></span> |<span data-ttu-id="489d9-362">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="489d9-362">String, Char[]</span></span> |
| <span data-ttu-id="489d9-363">skutečné</span><span class="sxs-lookup"><span data-stu-id="489d9-363">real</span></span> |<span data-ttu-id="489d9-364">Jeden</span><span class="sxs-lookup"><span data-stu-id="489d9-364">Single</span></span> |
| <span data-ttu-id="489d9-365">ROWVERSION</span><span class="sxs-lookup"><span data-stu-id="489d9-365">rowversion</span></span> |<span data-ttu-id="489d9-366">Byte]</span><span class="sxs-lookup"><span data-stu-id="489d9-366">Byte[]</span></span> |
| <span data-ttu-id="489d9-367">smalldatetime</span><span class="sxs-lookup"><span data-stu-id="489d9-367">smalldatetime</span></span> |<span data-ttu-id="489d9-368">Data a času</span><span class="sxs-lookup"><span data-stu-id="489d9-368">DateTime</span></span> |
| <span data-ttu-id="489d9-369">smallint</span><span class="sxs-lookup"><span data-stu-id="489d9-369">smallint</span></span> |<span data-ttu-id="489d9-370">Int16</span><span class="sxs-lookup"><span data-stu-id="489d9-370">Int16</span></span> |
| <span data-ttu-id="489d9-371">Smallmoney</span><span class="sxs-lookup"><span data-stu-id="489d9-371">smallmoney</span></span> |<span data-ttu-id="489d9-372">Decimal</span><span class="sxs-lookup"><span data-stu-id="489d9-372">Decimal</span></span> |
| <span data-ttu-id="489d9-373">SQL_VARIANT</span><span class="sxs-lookup"><span data-stu-id="489d9-373">sql_variant</span></span> |<span data-ttu-id="489d9-374">Objekt *</span><span class="sxs-lookup"><span data-stu-id="489d9-374">Object *</span></span> |
| <span data-ttu-id="489d9-375">Text</span><span class="sxs-lookup"><span data-stu-id="489d9-375">text</span></span> |<span data-ttu-id="489d9-376">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="489d9-376">String, Char[]</span></span> |
| <span data-ttu-id="489d9-377">time</span><span class="sxs-lookup"><span data-stu-id="489d9-377">time</span></span> |<span data-ttu-id="489d9-378">Časový interval</span><span class="sxs-lookup"><span data-stu-id="489d9-378">TimeSpan</span></span> |
| <span data-ttu-id="489d9-379">časové razítko</span><span class="sxs-lookup"><span data-stu-id="489d9-379">timestamp</span></span> |<span data-ttu-id="489d9-380">Byte]</span><span class="sxs-lookup"><span data-stu-id="489d9-380">Byte[]</span></span> |
| <span data-ttu-id="489d9-381">tinyint</span><span class="sxs-lookup"><span data-stu-id="489d9-381">tinyint</span></span> |<span data-ttu-id="489d9-382">Bajtů</span><span class="sxs-lookup"><span data-stu-id="489d9-382">Byte</span></span> |
| <span data-ttu-id="489d9-383">Typ UniqueIdentifier</span><span class="sxs-lookup"><span data-stu-id="489d9-383">uniqueidentifier</span></span> |<span data-ttu-id="489d9-384">Identifikátor GUID</span><span class="sxs-lookup"><span data-stu-id="489d9-384">Guid</span></span> |
| <span data-ttu-id="489d9-385">varbinary</span><span class="sxs-lookup"><span data-stu-id="489d9-385">varbinary</span></span> |<span data-ttu-id="489d9-386">Byte]</span><span class="sxs-lookup"><span data-stu-id="489d9-386">Byte[]</span></span> |
| <span data-ttu-id="489d9-387">varchar</span><span class="sxs-lookup"><span data-stu-id="489d9-387">varchar</span></span> |<span data-ttu-id="489d9-388">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="489d9-388">String, Char[]</span></span> |
| <span data-ttu-id="489d9-389">xml</span><span class="sxs-lookup"><span data-stu-id="489d9-389">xml</span></span> |<span data-ttu-id="489d9-390">XML</span><span class="sxs-lookup"><span data-stu-id="489d9-390">Xml</span></span> |

## <a name="map-source-to-sink-columns"></a><span data-ttu-id="489d9-391">Mapování zdroje jímky sloupců</span><span class="sxs-lookup"><span data-stu-id="489d9-391">Map source to sink columns</span></span>
<span data-ttu-id="489d9-392">Další informace o mapování sloupců v datové sadě zdrojového sloupce v datové sadě podřízený najdete v tématu [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="489d9-392">To learn about mapping columns in source dataset to columns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-copy"></a><span data-ttu-id="489d9-393">Opakovatelných kopie</span><span class="sxs-lookup"><span data-stu-id="489d9-393">Repeatable copy</span></span>
<span data-ttu-id="489d9-394">Při kopírování dat do databáze serveru SQL, připojí aktivitě kopírování dat do tabulky jímky ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="489d9-394">When copying data to SQL Server Database, the copy activity appends data to the sink table by default.</span></span> <span data-ttu-id="489d9-395">Místo toho provést UPSERT, najdete v tématu [Repeatable zapisovat do SqlSink](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) článku.</span><span class="sxs-lookup"><span data-stu-id="489d9-395">To perform an UPSERT instead,  See [Repeatable write to SqlSink](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) article.</span></span> 

<span data-ttu-id="489d9-396">Při kopírování dat z relačních dat ukládá, uvědomte si, aby se zabránilo neúmyslnému výstupy opakovatelnosti.</span><span class="sxs-lookup"><span data-stu-id="489d9-396">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="489d9-397">V Azure Data Factory může řez znovu ručně.</span><span class="sxs-lookup"><span data-stu-id="489d9-397">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="489d9-398">Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="489d9-398">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="489d9-399">Řez se znovu spustí, buď způsobem, musíte zajistit, že stejná data je pro čtení bez ohledu na to kolikrát řez je spustit.</span><span class="sxs-lookup"><span data-stu-id="489d9-399">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="489d9-400">V tématu [Repeatable číst z relačními zdroji](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="489d9-400">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="489d9-401">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="489d9-401">Performance and Tuning</span></span>
<span data-ttu-id="489d9-402">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) Další informace o klíčových faktorů, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby, jak optimalizovat ho.</span><span class="sxs-lookup"><span data-stu-id="489d9-402">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
