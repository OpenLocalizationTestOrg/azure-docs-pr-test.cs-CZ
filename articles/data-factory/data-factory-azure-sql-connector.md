---
title: aaaCopy data do/z Azure SQL Database | Microsoft Docs
description: "Zjistěte, jak toocopy data do/z Azure SQL Database pomocí Azure Data Factory."
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
ms.openlocfilehash: d2ff16191afb028da75699c5e4d0bb310538db0f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tooand-from-azure-sql-database-using-azure-data-factory"></a><span data-ttu-id="73207-103">Tooand kopírování dat z databáze SQL Azure pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="73207-103">Copy data tooand from Azure SQL Database using Azure Data Factory</span></span>
<span data-ttu-id="73207-104">Tento článek vysvětluje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove data tooand z databáze SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="73207-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data tooand from Azure SQL Database.</span></span> <span data-ttu-id="73207-105">Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="73207-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>  

## <a name="supported-scenarios"></a><span data-ttu-id="73207-106">Podporované scénáře</span><span class="sxs-lookup"><span data-stu-id="73207-106">Supported scenarios</span></span>
<span data-ttu-id="73207-107">Může kopírovat data **z databáze Azure SQL Database** toohello následující úložišť dat:</span><span class="sxs-lookup"><span data-stu-id="73207-107">You can copy data **from Azure SQL Database** toohello following data stores:</span></span>

[!INCLUDE [data-factory-supported-sinks](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="73207-108">Data můžete zkopírovat z hello následující úložišť dat **tooAzure SQL Database**:</span><span class="sxs-lookup"><span data-stu-id="73207-108">You can copy data from hello following data stores **tooAzure SQL Database**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

## <a name="supported-authentication-type"></a><span data-ttu-id="73207-109">Podporované typ ověřování</span><span class="sxs-lookup"><span data-stu-id="73207-109">Supported authentication type</span></span>
<span data-ttu-id="73207-110">Konektor služby Azure SQL Database podporuje základní ověřování.</span><span class="sxs-lookup"><span data-stu-id="73207-110">Azure SQL Database connector supports basic authentication.</span></span>

## <a name="getting-started"></a><span data-ttu-id="73207-111">Začínáme</span><span class="sxs-lookup"><span data-stu-id="73207-111">Getting started</span></span>
<span data-ttu-id="73207-112">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z Azure SQL Database pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="73207-112">You can create a pipeline with a copy activity that moves data to/from an Azure SQL Database by using different tools/APIs.</span></span>

<span data-ttu-id="73207-113">Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="73207-113">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="73207-114">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.</span><span class="sxs-lookup"><span data-stu-id="73207-114">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="73207-115">Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**.</span><span class="sxs-lookup"><span data-stu-id="73207-115">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="73207-116">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="73207-116">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="73207-117">Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:</span><span class="sxs-lookup"><span data-stu-id="73207-117">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span> 

1. <span data-ttu-id="73207-118">Vytvoření **objekt pro vytváření dat**.</span><span class="sxs-lookup"><span data-stu-id="73207-118">Create a **data factory**.</span></span> <span data-ttu-id="73207-119">Objekt pro vytváření dat může obsahovat jeden nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="73207-119">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="73207-120">Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="73207-120">Create **linked services** toolink input and output data stores tooyour data factory.</span></span> <span data-ttu-id="73207-121">Například pokud kopírování dat z Azure SQL database tooan úložiště objektů blob v Azure, vytvoříte dvě propojené služby toolink vašeho účtu úložiště Azure a Azure SQL databáze tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="73207-121">For example, if you are copying data from an Azure blob storage tooan Azure SQL database, you create two linked services toolink your Azure storage account and Azure SQL database tooyour data factory.</span></span> <span data-ttu-id="73207-122">Vlastnosti propojené služby, které jsou specifické tooAzure SQL Database, najdete v části [propojené vlastnosti služby](#linked-service-properties) části.</span><span class="sxs-lookup"><span data-stu-id="73207-122">For linked service properties that are specific tooAzure SQL Database, see [linked service properties](#linked-service-properties) section.</span></span> 
3. <span data-ttu-id="73207-123">Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="73207-123">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> <span data-ttu-id="73207-124">V příkladu hello uvedených v posledním kroku hello vytvořte kontejner objektů blob hello toospecify datovou sadu a složky, která obsahuje vstupní data hello.</span><span class="sxs-lookup"><span data-stu-id="73207-124">In hello example mentioned in hello last step, you create a dataset toospecify hello blob container and folder that contains hello input data.</span></span> <span data-ttu-id="73207-125">A vytvořte jinou datovou sadu toospecify hello SQL tabulkou v hello Azure SQL database, která obsahuje hello daty zkopírovanými z úložiště objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="73207-125">And, you create another dataset toospecify hello SQL table in hello Azure SQL database  that holds hello data copied from hello blob storage.</span></span> <span data-ttu-id="73207-126">Vlastnosti datové sady, které jsou specifické tooAzure Data Lake Store, naleznete v části [vlastnosti datové sady](#dataset-properties) části.</span><span class="sxs-lookup"><span data-stu-id="73207-126">For dataset properties that are specific tooAzure Data Lake Store, see [dataset properties](#dataset-properties) section.</span></span>
4. <span data-ttu-id="73207-127">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="73207-127">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="73207-128">V příkladu hello již bylo zmíněno dříve použijete BlobSource jako zdroj a SqlSink jako jímku pro aktivitu kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="73207-128">In hello example mentioned earlier, you use BlobSource as a source and SqlSink as a sink for hello copy activity.</span></span> <span data-ttu-id="73207-129">Podobně pokud zkopírujete z Azure SQL Database tooAzure úložiště objektů Blob, použijte SqlSource a BlobSink v aktivitě kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="73207-129">Similarly, if you are copying from Azure SQL Database tooAzure Blob Storage, you use SqlSource and BlobSink in hello copy activity.</span></span> <span data-ttu-id="73207-130">Vlastnosti aktivity kopírování, které jsou specifické tooAzure SQL Database, najdete v části [zkopírovat vlastnosti aktivity](#copy-activity-properties) části.</span><span class="sxs-lookup"><span data-stu-id="73207-130">For copy activity properties that are specific tooAzure SQL Database, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="73207-131">Podrobnosti o způsobu toouse úložiště dat jako zdroj nebo jímka klikněte na tlačítko hello odkaz v předchozí části hello pro data store.</span><span class="sxs-lookup"><span data-stu-id="73207-131">For details on how toouse a data store as a source or a sink, click hello link in hello previous section for your data store.</span></span>

<span data-ttu-id="73207-132">Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello).</span><span class="sxs-lookup"><span data-stu-id="73207-132">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="73207-133">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.</span><span class="sxs-lookup"><span data-stu-id="73207-133">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="73207-134">Ukázky s definicemi JSON entit služby Data Factory, které jsou používané toocopy data do/z Azure SQL Database, najdete v části [JSON příklady](#json-examples-for-copying-data-to-and-from-sql-database) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="73207-134">For samples with JSON definitions for Data Factory entities that are used toocopy data to/from an Azure SQL Database, see [JSON examples](#json-examples-for-copying-data-to-and-from-sql-database) section of this article.</span></span> 

<span data-ttu-id="73207-135">Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooAzure SQL Database:</span><span class="sxs-lookup"><span data-stu-id="73207-135">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooAzure SQL Database:</span></span> 

## <a name="linked-service-properties"></a><span data-ttu-id="73207-136">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="73207-136">Linked service properties</span></span>
<span data-ttu-id="73207-137">Azure SQL propojená služba propojuje služby Azure SQL database tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="73207-137">An Azure SQL linked service links an Azure SQL database tooyour data factory.</span></span> <span data-ttu-id="73207-138">Hello následující tabulka obsahuje popis JSON elementy konkrétní tooAzure SQL propojené služby.</span><span class="sxs-lookup"><span data-stu-id="73207-138">hello following table provides description for JSON elements specific tooAzure SQL linked service.</span></span>

| <span data-ttu-id="73207-139">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="73207-139">Property</span></span> | <span data-ttu-id="73207-140">Popis</span><span class="sxs-lookup"><span data-stu-id="73207-140">Description</span></span> | <span data-ttu-id="73207-141">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="73207-141">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="73207-142">type</span><span class="sxs-lookup"><span data-stu-id="73207-142">type</span></span> |<span data-ttu-id="73207-143">vlastnost typu Hello musí být nastavena na: **azuresqldatabase.**</span><span class="sxs-lookup"><span data-stu-id="73207-143">hello type property must be set to: **AzureSqlDatabase**</span></span> |<span data-ttu-id="73207-144">Ano</span><span class="sxs-lookup"><span data-stu-id="73207-144">Yes</span></span> |
| <span data-ttu-id="73207-145">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="73207-145">connectionString</span></span> |<span data-ttu-id="73207-146">Zadejte informace potřebné pro vlastnost connectionString hello tooconnect toohello Azure SQL Database instance.</span><span class="sxs-lookup"><span data-stu-id="73207-146">Specify information needed tooconnect toohello Azure SQL Database instance for hello connectionString property.</span></span> <span data-ttu-id="73207-147">Podporováno je pouze základní ověřování.</span><span class="sxs-lookup"><span data-stu-id="73207-147">Only basic authentication is supported.</span></span> |<span data-ttu-id="73207-148">Ano</span><span class="sxs-lookup"><span data-stu-id="73207-148">Yes</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="73207-149">Konfigurace [Firewall databáze Azure SQL](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) hello databázový server příliš[povolit serveru hello tooaccess Azure Services](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure).</span><span class="sxs-lookup"><span data-stu-id="73207-149">Configure [Azure SQL Database Firewall](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) hello database server too[allow Azure Services tooaccess hello server](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure).</span></span> <span data-ttu-id="73207-150">Kromě toho pokud kopírujete data tooAzure databáze SQL z mimo Azure včetně z místního zdroje dat se brána objekt pro vytváření dat, nakonfigurujte odpovídající rozsah IP adres pro hello počítač, který odesílá data tooAzure databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="73207-150">Additionally, if you are copying data tooAzure SQL Database from outside Azure including from on-premises data sources with data factory gateway, configure appropriate IP address range for hello machine that is sending data tooAzure SQL Database.</span></span>

## <a name="dataset-properties"></a><span data-ttu-id="73207-151">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="73207-151">Dataset properties</span></span>
<span data-ttu-id="73207-152">toospecify toorepresent datové sady vstupních nebo výstupních dat v databázi Azure SQL, nastavte vlastnost typu hello hello datovou sadu, která: **AzureSqlTable**.</span><span class="sxs-lookup"><span data-stu-id="73207-152">toospecify a dataset toorepresent input or output data in an Azure SQL database, you set hello type property of hello dataset to: **AzureSqlTable**.</span></span> <span data-ttu-id="73207-153">Sada hello **linkedServiceName** vlastnost hello datovou sadu toohello název hello Azure SQL, propojené služby.</span><span class="sxs-lookup"><span data-stu-id="73207-153">Set hello **linkedServiceName** property of hello dataset toohello name of hello Azure SQL linked service.</span></span>  

<span data-ttu-id="73207-154">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="73207-154">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="73207-155">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="73207-155">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="73207-156">část rámci typeProperties Hello se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello.</span><span class="sxs-lookup"><span data-stu-id="73207-156">hello typeProperties section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="73207-157">Hello **rámci typeProperties** části pro datovou sadu hello typu **AzureSqlTable** má hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="73207-157">hello **typeProperties** section for hello dataset of type **AzureSqlTable** has hello following properties:</span></span>

| <span data-ttu-id="73207-158">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="73207-158">Property</span></span> | <span data-ttu-id="73207-159">Popis</span><span class="sxs-lookup"><span data-stu-id="73207-159">Description</span></span> | <span data-ttu-id="73207-160">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="73207-160">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="73207-161">tableName</span><span class="sxs-lookup"><span data-stu-id="73207-161">tableName</span></span> |<span data-ttu-id="73207-162">Název hello tabulku nebo zobrazení hello Azure SQL Database instance, kterou propojená služba odkazuje.</span><span class="sxs-lookup"><span data-stu-id="73207-162">Name of hello table or view in hello Azure SQL Database instance that linked service refers to.</span></span> |<span data-ttu-id="73207-163">Ano</span><span class="sxs-lookup"><span data-stu-id="73207-163">Yes</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="73207-164">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="73207-164">Copy activity properties</span></span>
<span data-ttu-id="73207-165">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="73207-165">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="73207-166">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="73207-166">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

> [!NOTE]
> <span data-ttu-id="73207-167">Hello aktivity kopírování přijímá pouze jeden vstup a vytváří jenom jeden výstup.</span><span class="sxs-lookup"><span data-stu-id="73207-167">hello Copy Activity takes only one input and produces only one output.</span></span>

<span data-ttu-id="73207-168">Vzhledem k tomu, vlastnosti dostupné ve hello **rámci typeProperties** části hello aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="73207-168">Whereas, properties available in hello **typeProperties** section of hello activity vary with each activity type.</span></span> <span data-ttu-id="73207-169">Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="73207-169">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="73207-170">Pokud přesouváte data z databáze Azure SQL, nastavíte typ zdroje hello v aktivitě kopírování hello příliš**SqlSource**.</span><span class="sxs-lookup"><span data-stu-id="73207-170">If you are moving data from an Azure SQL database, you set hello source type in hello copy activity too**SqlSource**.</span></span> <span data-ttu-id="73207-171">Podobně pokud přesouváte data tooan Azure SQL database, nastavíte typ jímky hello v aktivitě kopírování hello příliš**SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="73207-171">Similarly, if you are moving data tooan Azure SQL database, you set hello sink type in hello copy activity too**SqlSink**.</span></span> <span data-ttu-id="73207-172">Tato část obsahuje seznam vlastností, které jsou podporované SqlSource a SqlSink.</span><span class="sxs-lookup"><span data-stu-id="73207-172">This section provides a list of properties supported by SqlSource and SqlSink.</span></span>

### <a name="sqlsource"></a><span data-ttu-id="73207-173">SqlSource</span><span class="sxs-lookup"><span data-stu-id="73207-173">SqlSource</span></span>
<span data-ttu-id="73207-174">Při aktivitě kopírování, pokud je zdroj hello typu **SqlSource**, hello následující vlastnosti jsou k dispozici v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="73207-174">In copy activity, when hello source is of type **SqlSource**, hello following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="73207-175">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="73207-175">Property</span></span> | <span data-ttu-id="73207-176">Popis</span><span class="sxs-lookup"><span data-stu-id="73207-176">Description</span></span> | <span data-ttu-id="73207-177">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="73207-177">Allowed values</span></span> | <span data-ttu-id="73207-178">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="73207-178">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="73207-179">sqlReaderQuery</span><span class="sxs-lookup"><span data-stu-id="73207-179">sqlReaderQuery</span></span> |<span data-ttu-id="73207-180">Použijte data tooread hello vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="73207-180">Use hello custom query tooread data.</span></span> |<span data-ttu-id="73207-181">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="73207-181">SQL query string.</span></span> <span data-ttu-id="73207-182">Příklad: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="73207-182">Example: `select * from MyTable`.</span></span> |<span data-ttu-id="73207-183">Ne</span><span class="sxs-lookup"><span data-stu-id="73207-183">No</span></span> |
| <span data-ttu-id="73207-184">sqlReaderStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="73207-184">sqlReaderStoredProcedureName</span></span> |<span data-ttu-id="73207-185">Název hello uložené procedury, která čte data z hello zdrojové tabulky.</span><span class="sxs-lookup"><span data-stu-id="73207-185">Name of hello stored procedure that reads data from hello source table.</span></span> |<span data-ttu-id="73207-186">Název hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="73207-186">Name of hello stored procedure.</span></span> <span data-ttu-id="73207-187">Hello poslední příkaz jazyka SQL musí být příkaz SELECT v hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="73207-187">hello last SQL statement must be a SELECT statement in hello stored procedure.</span></span> |<span data-ttu-id="73207-188">Ne</span><span class="sxs-lookup"><span data-stu-id="73207-188">No</span></span> |
| <span data-ttu-id="73207-189">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="73207-189">storedProcedureParameters</span></span> |<span data-ttu-id="73207-190">Parametry pro hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="73207-190">Parameters for hello stored procedure.</span></span> |<span data-ttu-id="73207-191">Páry název/hodnota.</span><span class="sxs-lookup"><span data-stu-id="73207-191">Name/value pairs.</span></span> <span data-ttu-id="73207-192">Názvy a malá a velká písmena parametry musí odpovídat názvům hello a malá a velká písmena parametry hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="73207-192">Names and casing of parameters must match hello names and casing of hello stored procedure parameters.</span></span> |<span data-ttu-id="73207-193">Ne</span><span class="sxs-lookup"><span data-stu-id="73207-193">No</span></span> |

<span data-ttu-id="73207-194">Pokud hello **sqlReaderQuery** je zadán pro hello SqlSource, hello aktivity kopírování spouští tento dotaz hello Azure SQL Database zdrojová tooget hello data.</span><span class="sxs-lookup"><span data-stu-id="73207-194">If hello **sqlReaderQuery** is specified for hello SqlSource, hello Copy Activity runs this query against hello Azure SQL Database source tooget hello data.</span></span> <span data-ttu-id="73207-195">Alternativně můžete zadat uložené procedury zadáním hello **sqlReaderStoredProcedureName** a **storedProcedureParameters** (Pokud hello uložená procedura používá parametry).</span><span class="sxs-lookup"><span data-stu-id="73207-195">Alternatively, you can specify a stored procedure by specifying hello **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if hello stored procedure takes parameters).</span></span>

<span data-ttu-id="73207-196">Pokud nezadáte sqlReaderQuery nebo sqlReaderStoredProcedureName, hello sloupce definované v části struktura hello hello sady dat JSON jsou použité toobuild dotazu (`select column1, column2 from mytable`) toorun proti hello Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="73207-196">If you do not specify either sqlReaderQuery or sqlReaderStoredProcedureName, hello columns defined in hello structure section of hello dataset JSON are used toobuild a query (`select column1, column2 from mytable`) toorun against hello Azure SQL Database.</span></span> <span data-ttu-id="73207-197">Pokud definice datové sady hello nemá hello struktura, vyberou se všechny sloupce z tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="73207-197">If hello dataset definition does not have hello structure, all columns are selected from hello table.</span></span>

> [!NOTE]
> <span data-ttu-id="73207-198">Při použití **sqlReaderStoredProcedureName**, stále potřebujete toospecify hodnotu pro hello **tableName** vlastnost v datové sadě hello JSON.</span><span class="sxs-lookup"><span data-stu-id="73207-198">When you use **sqlReaderStoredProcedureName**, you still need toospecify a value for hello **tableName** property in hello dataset JSON.</span></span> <span data-ttu-id="73207-199">Neexistují žádné ověření, ale adresovat této tabulky.</span><span class="sxs-lookup"><span data-stu-id="73207-199">There are no validations performed against this table though.</span></span>
>
>

### <a name="sqlsource-example"></a><span data-ttu-id="73207-200">Příklad SqlSource</span><span class="sxs-lookup"><span data-stu-id="73207-200">SqlSource example</span></span>

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

<span data-ttu-id="73207-201">**definice Hello uložené procedury:**</span><span class="sxs-lookup"><span data-stu-id="73207-201">**hello stored procedure definition:**</span></span>

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

### <a name="sqlsink"></a><span data-ttu-id="73207-202">SqlSink</span><span class="sxs-lookup"><span data-stu-id="73207-202">SqlSink</span></span>
<span data-ttu-id="73207-203">**SqlSink** podporuje hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="73207-203">**SqlSink** supports hello following properties:</span></span>

| <span data-ttu-id="73207-204">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="73207-204">Property</span></span> | <span data-ttu-id="73207-205">Popis</span><span class="sxs-lookup"><span data-stu-id="73207-205">Description</span></span> | <span data-ttu-id="73207-206">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="73207-206">Allowed values</span></span> | <span data-ttu-id="73207-207">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="73207-207">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="73207-208">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="73207-208">writeBatchTimeout</span></span> |<span data-ttu-id="73207-209">Doba pro toocomplete operaci vložení dávky hello Počkejte, než vyprší časový limit.</span><span class="sxs-lookup"><span data-stu-id="73207-209">Wait time for hello batch insert operation toocomplete before it times out.</span></span> |<span data-ttu-id="73207-210">Časový interval</span><span class="sxs-lookup"><span data-stu-id="73207-210">timespan</span></span><br/><br/> <span data-ttu-id="73207-211">Příklad: "00: 30:00" (30 minut).</span><span class="sxs-lookup"><span data-stu-id="73207-211">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="73207-212">Ne</span><span class="sxs-lookup"><span data-stu-id="73207-212">No</span></span> |
| <span data-ttu-id="73207-213">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="73207-213">writeBatchSize</span></span> |<span data-ttu-id="73207-214">Pokud velikost vyrovnávací paměti hello dosáhne writeBatchSize vkládá data do tabulky SQL hello.</span><span class="sxs-lookup"><span data-stu-id="73207-214">Inserts data into hello SQL table when hello buffer size reaches writeBatchSize.</span></span> |<span data-ttu-id="73207-215">Celé číslo (počet řádků)</span><span class="sxs-lookup"><span data-stu-id="73207-215">Integer (number of rows)</span></span> |<span data-ttu-id="73207-216">Ne (výchozí: 10000)</span><span class="sxs-lookup"><span data-stu-id="73207-216">No (default: 10000)</span></span> |
| <span data-ttu-id="73207-217">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="73207-217">sqlWriterCleanupScript</span></span> |<span data-ttu-id="73207-218">Zadejte dotaz aktivity kopírování tooexecute tak, aby se vyčistit data určitý řez.</span><span class="sxs-lookup"><span data-stu-id="73207-218">Specify a query for Copy Activity tooexecute such that data of a specific slice is cleaned up.</span></span> <span data-ttu-id="73207-219">Další informace najdete v tématu [opakovatelných kopie](#repeatable-copy).</span><span class="sxs-lookup"><span data-stu-id="73207-219">For more information, see [repeatable copy](#repeatable-copy).</span></span> |<span data-ttu-id="73207-220">Příkaz dotazu.</span><span class="sxs-lookup"><span data-stu-id="73207-220">A query statement.</span></span> |<span data-ttu-id="73207-221">Ne</span><span class="sxs-lookup"><span data-stu-id="73207-221">No</span></span> |
| <span data-ttu-id="73207-222">sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="73207-222">sliceIdentifierColumnName</span></span> |<span data-ttu-id="73207-223">Zadejte název sloupce pro aktivitu kopírování toofill s identifikátorem automaticky generovány řez, což je použité tooclean data určitý řez, pokud znovu spustit.</span><span class="sxs-lookup"><span data-stu-id="73207-223">Specify a column name for Copy Activity toofill with auto generated slice identifier, which is used tooclean up data of a specific slice when rerun.</span></span> <span data-ttu-id="73207-224">Další informace najdete v tématu [opakovatelných kopie](#repeatable-copy).</span><span class="sxs-lookup"><span data-stu-id="73207-224">For more information, see [repeatable copy](#repeatable-copy).</span></span> |<span data-ttu-id="73207-225">Název sloupce sloupce s datovým typem binary(32).</span><span class="sxs-lookup"><span data-stu-id="73207-225">Column name of a column with data type of binary(32).</span></span> |<span data-ttu-id="73207-226">Ne</span><span class="sxs-lookup"><span data-stu-id="73207-226">No</span></span> |
| <span data-ttu-id="73207-227">sqlWriterStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="73207-227">sqlWriterStoredProcedureName</span></span> |<span data-ttu-id="73207-228">Název hello uložené procedury upserts (aktualizace nebo vložení) dat do cílové tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="73207-228">Name of hello stored procedure that upserts (updates/inserts) data into hello target table.</span></span> |<span data-ttu-id="73207-229">Název hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="73207-229">Name of hello stored procedure.</span></span> |<span data-ttu-id="73207-230">Ne</span><span class="sxs-lookup"><span data-stu-id="73207-230">No</span></span> |
| <span data-ttu-id="73207-231">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="73207-231">storedProcedureParameters</span></span> |<span data-ttu-id="73207-232">Parametry pro hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="73207-232">Parameters for hello stored procedure.</span></span> |<span data-ttu-id="73207-233">Páry název/hodnota.</span><span class="sxs-lookup"><span data-stu-id="73207-233">Name/value pairs.</span></span> <span data-ttu-id="73207-234">Názvy a malá a velká písmena parametry musí odpovídat názvům hello a malá a velká písmena parametry hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="73207-234">Names and casing of parameters must match hello names and casing of hello stored procedure parameters.</span></span> |<span data-ttu-id="73207-235">Ne</span><span class="sxs-lookup"><span data-stu-id="73207-235">No</span></span> |
| <span data-ttu-id="73207-236">sqlWriterTableType</span><span class="sxs-lookup"><span data-stu-id="73207-236">sqlWriterTableType</span></span> |<span data-ttu-id="73207-237">Zadejte toobe tabulku typu název používá v hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="73207-237">Specify a table type name toobe used in hello stored procedure.</span></span> <span data-ttu-id="73207-238">Aktivita kopírování zpřístupní přesouvání dat hello v dočasné tabulce s tímto typem tabulky.</span><span class="sxs-lookup"><span data-stu-id="73207-238">Copy activity makes hello data being moved available in a temp table with this table type.</span></span> <span data-ttu-id="73207-239">Uložená procedura kódu můžete pak sloučit data hello kopírovány s existujícími daty.</span><span class="sxs-lookup"><span data-stu-id="73207-239">Stored procedure code can then merge hello data being copied with existing data.</span></span> |<span data-ttu-id="73207-240">Zadejte název tabulky.</span><span class="sxs-lookup"><span data-stu-id="73207-240">A table type name.</span></span> |<span data-ttu-id="73207-241">Ne</span><span class="sxs-lookup"><span data-stu-id="73207-241">No</span></span> |

#### <a name="sqlsink-example"></a><span data-ttu-id="73207-242">Příklad SqlSink</span><span class="sxs-lookup"><span data-stu-id="73207-242">SqlSink example</span></span>

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

## <a name="json-examples-for-copying-data-tooand-from-sql-database"></a><span data-ttu-id="73207-243">Příklady JSON pro kopírování data tooand z databáze SQL</span><span class="sxs-lookup"><span data-stu-id="73207-243">JSON examples for copying data tooand from SQL Database</span></span>
<span data-ttu-id="73207-244">Hello následující příklady poskytují definice JSON ukázka používané toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="73207-244">hello following examples provide sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="73207-245">Ukazují jak toocopy tooand dat z Azure SQL Database a Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="73207-245">They show how toocopy data tooand from Azure SQL Database and Azure Blob Storage.</span></span> <span data-ttu-id="73207-246">Nicméně je možné zkopírovat data **přímo** ze všech zdrojů tooany z hello jímky uvádí [zde](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="73207-246">However, data can be copied **directly** from any of sources tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>

### <a name="example-copy-data-from-azure-sql-database-tooazure-blob"></a><span data-ttu-id="73207-247">Příklad: Kopírování dat z Azure SQL Database tooAzure objektů Blob</span><span class="sxs-lookup"><span data-stu-id="73207-247">Example: Copy data from Azure SQL Database tooAzure Blob</span></span>
<span data-ttu-id="73207-248">Hello stejné definuje hello následující entity služby Data Factory:</span><span class="sxs-lookup"><span data-stu-id="73207-248">hello same defines hello following Data Factory entities:</span></span>

1. <span data-ttu-id="73207-249">Propojené služby typu [azuresqldatabase](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="73207-249">A linked service of type [AzureSqlDatabase](#linked-service-properties).</span></span>
2. <span data-ttu-id="73207-250">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="73207-250">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="73207-251">Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureSqlTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="73207-251">An input [dataset](data-factory-create-datasets.md) of type [AzureSqlTable](#dataset-properties).</span></span>
4. <span data-ttu-id="73207-252">Výstup [datovou sadu](data-factory-create-datasets.md) typu [objektů Blob v Azure](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="73207-252">An output [dataset](data-factory-create-datasets.md) of type [Azure Blob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="73207-253">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [SqlSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="73207-253">A [pipeline](data-factory-create-pipelines.md) with a Copy activity that uses [SqlSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="73207-254">Ukázka Hello zkopíruje data časové řady (hodinový, denní, atd.) z tabulky v objektu blob tooa databáze Azure SQL každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="73207-254">hello sample copies time-series data (hourly, daily, etc.) from a table in Azure SQL database tooa blob every hour.</span></span> <span data-ttu-id="73207-255">Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.</span><span class="sxs-lookup"><span data-stu-id="73207-255">hello JSON properties used in these samples are described in sections following hello samples.</span></span>  

<span data-ttu-id="73207-256">**Azure SQL Database propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="73207-256">**Azure SQL Database linked service:**</span></span>

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
<span data-ttu-id="73207-257">V tématu hello [propojená služba Azure SQL](#linked-service) části hello seznamu vlastností podporuje tuto propojenou službu.</span><span class="sxs-lookup"><span data-stu-id="73207-257">See hello [Azure SQL Linked Service](#linked-service) section for hello list of properties supported by this linked service.</span></span>

<span data-ttu-id="73207-258">**Propojená služba Azure Blob storage:**</span><span class="sxs-lookup"><span data-stu-id="73207-258">**Azure Blob storage linked service:**</span></span>

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
<span data-ttu-id="73207-259">V tématu hello [objektů Blob v Azure](data-factory-azure-blob-connector.md#azure-storage-linked-service) článku hello seznamu vlastností podporuje tuto propojenou službu.</span><span class="sxs-lookup"><span data-stu-id="73207-259">See hello [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) article for hello list of properties supported by this linked service.</span></span>


<span data-ttu-id="73207-260">**Azure SQL vstupní datové sady:**</span><span class="sxs-lookup"><span data-stu-id="73207-260">**Azure SQL input dataset:**</span></span>

<span data-ttu-id="73207-261">Ukázka Hello předpokládá jste vytvořili tabulku "MyTable" v Azure SQL a obsahuje sloupec s názvem "timestampcolumn" pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="73207-261">hello sample assumes you have created a table “MyTable” in Azure SQL and it contains a column called “timestampcolumn” for time series data.</span></span>

<span data-ttu-id="73207-262">Nastavení "externí": "PRAVDA" informuje služby Azure Data Factory hello tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="73207-262">Setting “external”: ”true” informs hello Azure Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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

<span data-ttu-id="73207-263">V tématu hello [vlastnosti typu datové sady Azure SQL](#dataset) části hello seznamu vlastností nepodporuje tento typ dataset.</span><span class="sxs-lookup"><span data-stu-id="73207-263">See hello [Azure SQL dataset type properties](#dataset) section for hello list of properties supported by this dataset type.</span></span>  

<span data-ttu-id="73207-264">**Azure Blob výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="73207-264">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="73207-265">Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="73207-265">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="73207-266">Cesta ke složce Hello pro objekt blob hello je vyhodnocován dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="73207-266">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="73207-267">Cesta ke složce Hello používá rok, měsíc, den a čas části hello počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="73207-267">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

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
<span data-ttu-id="73207-268">V tématu hello [vlastnosti typu datovou sadu objektu Blob Azure](data-factory-azure-blob-connector.md#dataset-properties) části hello seznamu vlastností nepodporuje tento typ dataset.</span><span class="sxs-lookup"><span data-stu-id="73207-268">See hello [Azure Blob dataset type properties](data-factory-azure-blob-connector.md#dataset-properties) section for hello list of properties supported by this dataset type.</span></span>  

<span data-ttu-id="73207-269">**Aktivita kopírování v kanálu s SQL zdrojový a podřízený objekt Blob:**</span><span class="sxs-lookup"><span data-stu-id="73207-269">**A copy activity in a pipeline with SQL source and Blob sink:**</span></span>

<span data-ttu-id="73207-270">Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="73207-270">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="73207-271">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**SqlSource** a **podřízený** je typ nastaven příliš**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="73207-271">In hello pipeline JSON definition, hello **source** type is set too**SqlSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="73207-272">Dotaz SQL Hello zadaný pro hello **SqlReaderQuery** vlastnost vybere hello data v hello za hodinu toocopy.</span><span class="sxs-lookup"><span data-stu-id="73207-272">hello SQL query specified for hello **SqlReaderQuery** property selects hello data in hello past hour toocopy.</span></span>

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
<span data-ttu-id="73207-273">V příkladu hello **sqlReaderQuery** pro hello SqlSource je zadána.</span><span class="sxs-lookup"><span data-stu-id="73207-273">In hello example, **sqlReaderQuery** is specified for hello SqlSource.</span></span> <span data-ttu-id="73207-274">Aktivita kopírování Hello spouští tento dotaz hello Azure SQL Database zdrojová tooget hello data.</span><span class="sxs-lookup"><span data-stu-id="73207-274">hello Copy Activity runs this query against hello Azure SQL Database source tooget hello data.</span></span> <span data-ttu-id="73207-275">Alternativně můžete zadat uložené procedury zadáním hello **sqlReaderStoredProcedureName** a **storedProcedureParameters** (Pokud hello uložená procedura používá parametry).</span><span class="sxs-lookup"><span data-stu-id="73207-275">Alternatively, you can specify a stored procedure by specifying hello **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if hello stored procedure takes parameters).</span></span>

<span data-ttu-id="73207-276">Pokud nezadáte sqlReaderQuery nebo sqlReaderStoredProcedureName, hello sloupce definovaný v oddílu Struktura hello hello sady dat JSON jsou použité toobuild toorun dotaz proti hello Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="73207-276">If you do not specify either sqlReaderQuery or sqlReaderStoredProcedureName, hello columns defined in hello structure section of hello dataset JSON are used toobuild a query toorun against hello Azure SQL Database.</span></span> <span data-ttu-id="73207-277">Například: `select column1, column2 from mytable`.</span><span class="sxs-lookup"><span data-stu-id="73207-277">For example: `select column1, column2 from mytable`.</span></span> <span data-ttu-id="73207-278">Pokud definice datové sady hello nemá hello struktura, vyberou se všechny sloupce z tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="73207-278">If hello dataset definition does not have hello structure, all columns are selected from hello table.</span></span>

<span data-ttu-id="73207-279">V tématu hello [zdroje Sql](#sqlsource) části a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) hello seznam vlastnostech podporovaných zprostředkovatelem SqlSource a BlobSink.</span><span class="sxs-lookup"><span data-stu-id="73207-279">See hello [Sql Source](#sqlsource) section and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) for hello list of properties supported by SqlSource and BlobSink.</span></span>

### <a name="example-copy-data-from-azure-blob-tooazure-sql-database"></a><span data-ttu-id="73207-280">Příklad: Kopírování dat z Azure Blob tooAzure databáze SQL</span><span class="sxs-lookup"><span data-stu-id="73207-280">Example: Copy data from Azure Blob tooAzure SQL Database</span></span>
<span data-ttu-id="73207-281">Ukázka Hello definuje hello následující entity služby Data Factory:</span><span class="sxs-lookup"><span data-stu-id="73207-281">hello sample defines hello following Data Factory entities:</span></span>  

1. <span data-ttu-id="73207-282">Propojené služby typu [azuresqldatabase](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="73207-282">A linked service of type [AzureSqlDatabase](#linked-service-properties).</span></span>
2. <span data-ttu-id="73207-283">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="73207-283">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="73207-284">Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="73207-284">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="73207-285">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureSqlTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="73207-285">An output [dataset](data-factory-create-datasets.md) of type [AzureSqlTable](#dataset-properties).</span></span>
5. <span data-ttu-id="73207-286">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) a [SqlSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="73207-286">A [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) and [SqlSink](#copy-activity-properties).</span></span>

<span data-ttu-id="73207-287">Ukázka Hello zkopíruje data časové řady (hodinový, denní, atd.) z Azure blob tooa tabulky v databázi Azure SQL každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="73207-287">hello sample copies time-series data (hourly, daily, etc.) from Azure blob tooa table in Azure SQL database every hour.</span></span> <span data-ttu-id="73207-288">Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.</span><span class="sxs-lookup"><span data-stu-id="73207-288">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="73207-289">**Azure SQL propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="73207-289">**Azure SQL linked service:**</span></span>

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
<span data-ttu-id="73207-290">V tématu hello [propojená služba Azure SQL](#linked-service) části hello seznamu vlastností podporuje tuto propojenou službu.</span><span class="sxs-lookup"><span data-stu-id="73207-290">See hello [Azure SQL Linked Service](#linked-service) section for hello list of properties supported by this linked service.</span></span>

<span data-ttu-id="73207-291">**Propojená služba Azure Blob storage:**</span><span class="sxs-lookup"><span data-stu-id="73207-291">**Azure Blob storage linked service:**</span></span>

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
<span data-ttu-id="73207-292">V tématu hello [objektů Blob v Azure](data-factory-azure-blob-connector.md#azure-storage-linked-service) článku hello seznamu vlastností podporuje tuto propojenou službu.</span><span class="sxs-lookup"><span data-stu-id="73207-292">See hello [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) article for hello list of properties supported by this linked service.</span></span>


<span data-ttu-id="73207-293">**Azure vstupní datovou sadu objektu Blob:**</span><span class="sxs-lookup"><span data-stu-id="73207-293">**Azure Blob input dataset:**</span></span>

<span data-ttu-id="73207-294">Data je převzata z nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="73207-294">Data is picked up from a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="73207-295">název a cesta k souboru složky Hello pro objekt blob hello se vyhodnocují dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="73207-295">hello folder path and file name for hello blob are dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="73207-296">Cesta ke složce Hello používá rok, měsíc a den součástí hello počáteční čas a název souboru používá hello hodinu součástí hello počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="73207-296">hello folder path uses year, month, and day part of hello start time and file name uses hello hour part of hello start time.</span></span> <span data-ttu-id="73207-297">"externí": "PRAVDA" nastavení informuje hello služba Data Factory, tato tabulka je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="73207-297">“external”: “true” setting informs hello Data Factory service that this table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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
<span data-ttu-id="73207-298">V tématu hello [vlastnosti typu datovou sadu objektu Blob Azure](data-factory-azure-blob-connector.md#dataset-properties) části hello seznamu vlastností nepodporuje tento typ dataset.</span><span class="sxs-lookup"><span data-stu-id="73207-298">See hello [Azure Blob dataset type properties](data-factory-azure-blob-connector.md#dataset-properties) section for hello list of properties supported by this dataset type.</span></span>

<span data-ttu-id="73207-299">**Azure SQL Database výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="73207-299">**Azure SQL Database output dataset:**</span></span>

<span data-ttu-id="73207-300">Ukázka Hello zkopíruje data tooa tabulku s názvem "MyTable" v Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="73207-300">hello sample copies data tooa table named “MyTable” in Azure SQL.</span></span> <span data-ttu-id="73207-301">Vytvoření tabulky hello v Azure SQL s hello stejný počet sloupců, podle očekávání toocontain soubor Blob CSV hello.</span><span class="sxs-lookup"><span data-stu-id="73207-301">Create hello table in Azure SQL with hello same number of columns as you expect hello Blob CSV file toocontain.</span></span> <span data-ttu-id="73207-302">Přidávání řádků tabulky toohello každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="73207-302">New rows are added toohello table every hour.</span></span>

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
<span data-ttu-id="73207-303">V tématu hello [vlastnosti typu datové sady Azure SQL](#dataset) části hello seznamu vlastností nepodporuje tento typ dataset.</span><span class="sxs-lookup"><span data-stu-id="73207-303">See hello [Azure SQL dataset type properties](#dataset) section for hello list of properties supported by this dataset type.</span></span>

<span data-ttu-id="73207-304">**Aktivita kopírování v kanálu s Blob zdroj a jímka SQL:**</span><span class="sxs-lookup"><span data-stu-id="73207-304">**A copy activity in a pipeline with Blob source and SQL sink:**</span></span>

<span data-ttu-id="73207-305">Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="73207-305">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="73207-306">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**BlobSource** a **podřízený** je typ nastaven příliš**SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="73207-306">In hello pipeline JSON definition, hello **source** type is set too**BlobSource** and **sink** type is set too**SqlSink**.</span></span>

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
<span data-ttu-id="73207-307">V tématu hello [jímku Sql](#sqlsink) části a [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) hello seznam vlastnostech podporovaných zprostředkovatelem SqlSink a BlobSource.</span><span class="sxs-lookup"><span data-stu-id="73207-307">See hello [Sql Sink](#sqlsink) section and [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) for hello list of properties supported by SqlSink and BlobSource.</span></span>

## <a name="identity-columns-in-hello-target-database"></a><span data-ttu-id="73207-308">Sloupce identity v hello cílová databáze</span><span class="sxs-lookup"><span data-stu-id="73207-308">Identity columns in hello target database</span></span>
<span data-ttu-id="73207-309">Tato část poskytuje příklad pro kopírování dat ze zdrojové tabulky bez identity sloupec tooa cílové tabulky se sloupcem identity.</span><span class="sxs-lookup"><span data-stu-id="73207-309">This section provides an example for copying data from a source table without an identity column tooa destination table with an identity column.</span></span>

<span data-ttu-id="73207-310">**Zdrojová tabulka:**</span><span class="sxs-lookup"><span data-stu-id="73207-310">**Source table:**</span></span>

```SQL
create table dbo.SourceTbl
(
       name varchar(100),
       age int
)
```
<span data-ttu-id="73207-311">**Cílové tabulky:**</span><span class="sxs-lookup"><span data-stu-id="73207-311">**Destination table:**</span></span>

```SQL
create table dbo.TargetTbl
(
       identifier int identity(1,1),
       name varchar(100),
       age int
)
```
<span data-ttu-id="73207-312">Všimněte si, že hello cílová tabulka obsahuje sloupec identity.</span><span class="sxs-lookup"><span data-stu-id="73207-312">Notice that hello target table has an identity column.</span></span>

<span data-ttu-id="73207-313">**Definice JSON datové sady zdroje**</span><span class="sxs-lookup"><span data-stu-id="73207-313">**Source dataset JSON definition**</span></span>

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
<span data-ttu-id="73207-314">**Cílový definici JSON datové sady**</span><span class="sxs-lookup"><span data-stu-id="73207-314">**Destination dataset JSON definition**</span></span>

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

<span data-ttu-id="73207-315">Všimněte si, že jako zdrojové a cílové tabulky jiné schéma (cíl má sloupec s identitou).</span><span class="sxs-lookup"><span data-stu-id="73207-315">Notice that as your source and target table have different schema (target has an additional column with identity).</span></span> <span data-ttu-id="73207-316">V tomto scénáři budete potřebovat toospecify **struktura** vlastnost v definici datové sady cíl hello, který neobsahuje sloupec identity hello.</span><span class="sxs-lookup"><span data-stu-id="73207-316">In this scenario, you need toospecify **structure** property in hello target dataset definition, which doesn’t include hello identity column.</span></span>

## <a name="invoke-stored-procedure-from-sql-sink"></a><span data-ttu-id="73207-317">Volání uložené procedury jímku SQL</span><span class="sxs-lookup"><span data-stu-id="73207-317">Invoke stored procedure from SQL sink</span></span>
<span data-ttu-id="73207-318">Příklad volání uložené procedury z jímku SQL při aktivitě kopírování kanálu, naleznete v části [vyvolat uloženou proceduru SQL jímka v aktivitě kopírování](data-factory-invoke-stored-procedure-from-copy-activity.md) článku.</span><span class="sxs-lookup"><span data-stu-id="73207-318">For an example of invoking a stored procedure from SQL sink in a copy activity of a pipeline, see [Invoke stored procedure for SQL sink in copy activity](data-factory-invoke-stored-procedure-from-copy-activity.md) article.</span></span> 

## <a name="type-mapping-for-azure-sql-database"></a><span data-ttu-id="73207-319">Mapování typu pro databázi SQL Azure</span><span class="sxs-lookup"><span data-stu-id="73207-319">Type mapping for Azure SQL Database</span></span>
<span data-ttu-id="73207-320">Jak je uvedeno v hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů toosink typy zdroje s hello následující přístup krok 2:</span><span class="sxs-lookup"><span data-stu-id="73207-320">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article Copy activity performs automatic type conversions from source types toosink types with hello following 2-step approach:</span></span>

1. <span data-ttu-id="73207-321">Převod z typu too.NET typy nativní zdroje</span><span class="sxs-lookup"><span data-stu-id="73207-321">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="73207-322">Převést typ jímky toonative typ rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="73207-322">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="73207-323">Při přesunu data tooand z databáze Azure SQL Database, hello následující mapování se používají z typu too.NET typ SQL a naopak.</span><span class="sxs-lookup"><span data-stu-id="73207-323">When moving data tooand from Azure SQL Database, hello following mappings are used from SQL type too.NET type and vice versa.</span></span> <span data-ttu-id="73207-324">mapování Hello je stejný jako hello mapování datového typu aplikace SQL Server pro technologii ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="73207-324">hello mapping is same as hello SQL Server Data Type Mapping for ADO.NET.</span></span>

| <span data-ttu-id="73207-325">Typ databázového stroje SQL Server</span><span class="sxs-lookup"><span data-stu-id="73207-325">SQL Server Database Engine type</span></span> | <span data-ttu-id="73207-326">Typ rozhraní .NET framework</span><span class="sxs-lookup"><span data-stu-id="73207-326">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="73207-327">bigint</span><span class="sxs-lookup"><span data-stu-id="73207-327">bigint</span></span> |<span data-ttu-id="73207-328">Int64</span><span class="sxs-lookup"><span data-stu-id="73207-328">Int64</span></span> |
| <span data-ttu-id="73207-329">Binární</span><span class="sxs-lookup"><span data-stu-id="73207-329">binary</span></span> |<span data-ttu-id="73207-330">Byte]</span><span class="sxs-lookup"><span data-stu-id="73207-330">Byte[]</span></span> |
| <span data-ttu-id="73207-331">Bit</span><span class="sxs-lookup"><span data-stu-id="73207-331">bit</span></span> |<span data-ttu-id="73207-332">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="73207-332">Boolean</span></span> |
| <span data-ttu-id="73207-333">Char</span><span class="sxs-lookup"><span data-stu-id="73207-333">char</span></span> |<span data-ttu-id="73207-334">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="73207-334">String, Char[]</span></span> |
| <span data-ttu-id="73207-335">Datum</span><span class="sxs-lookup"><span data-stu-id="73207-335">date</span></span> |<span data-ttu-id="73207-336">Data a času</span><span class="sxs-lookup"><span data-stu-id="73207-336">DateTime</span></span> |
| <span data-ttu-id="73207-337">Data a času</span><span class="sxs-lookup"><span data-stu-id="73207-337">Datetime</span></span> |<span data-ttu-id="73207-338">Data a času</span><span class="sxs-lookup"><span data-stu-id="73207-338">DateTime</span></span> |
| <span data-ttu-id="73207-339">datetime2</span><span class="sxs-lookup"><span data-stu-id="73207-339">datetime2</span></span> |<span data-ttu-id="73207-340">Data a času</span><span class="sxs-lookup"><span data-stu-id="73207-340">DateTime</span></span> |
| <span data-ttu-id="73207-341">Datový typ DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="73207-341">Datetimeoffset</span></span> |<span data-ttu-id="73207-342">Datový typ DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="73207-342">DateTimeOffset</span></span> |
| <span data-ttu-id="73207-343">Decimal</span><span class="sxs-lookup"><span data-stu-id="73207-343">Decimal</span></span> |<span data-ttu-id="73207-344">Decimal</span><span class="sxs-lookup"><span data-stu-id="73207-344">Decimal</span></span> |
| <span data-ttu-id="73207-345">Atribut FILESTREAM (varbinary(max))</span><span class="sxs-lookup"><span data-stu-id="73207-345">FILESTREAM attribute (varbinary(max))</span></span> |<span data-ttu-id="73207-346">Byte]</span><span class="sxs-lookup"><span data-stu-id="73207-346">Byte[]</span></span> |
| <span data-ttu-id="73207-347">Plovoucí desetinná čárka</span><span class="sxs-lookup"><span data-stu-id="73207-347">Float</span></span> |<span data-ttu-id="73207-348">Double</span><span class="sxs-lookup"><span data-stu-id="73207-348">Double</span></span> |
| <span data-ttu-id="73207-349">Bitové kopie</span><span class="sxs-lookup"><span data-stu-id="73207-349">image</span></span> |<span data-ttu-id="73207-350">Byte]</span><span class="sxs-lookup"><span data-stu-id="73207-350">Byte[]</span></span> |
| <span data-ttu-id="73207-351">celá čísla</span><span class="sxs-lookup"><span data-stu-id="73207-351">int</span></span> |<span data-ttu-id="73207-352">Int32</span><span class="sxs-lookup"><span data-stu-id="73207-352">Int32</span></span> |
| <span data-ttu-id="73207-353">peníze</span><span class="sxs-lookup"><span data-stu-id="73207-353">money</span></span> |<span data-ttu-id="73207-354">Decimal</span><span class="sxs-lookup"><span data-stu-id="73207-354">Decimal</span></span> |
| <span data-ttu-id="73207-355">nchar</span><span class="sxs-lookup"><span data-stu-id="73207-355">nchar</span></span> |<span data-ttu-id="73207-356">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="73207-356">String, Char[]</span></span> |
| <span data-ttu-id="73207-357">ntext</span><span class="sxs-lookup"><span data-stu-id="73207-357">ntext</span></span> |<span data-ttu-id="73207-358">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="73207-358">String, Char[]</span></span> |
| <span data-ttu-id="73207-359">číselné</span><span class="sxs-lookup"><span data-stu-id="73207-359">numeric</span></span> |<span data-ttu-id="73207-360">Decimal</span><span class="sxs-lookup"><span data-stu-id="73207-360">Decimal</span></span> |
| <span data-ttu-id="73207-361">nvarchar</span><span class="sxs-lookup"><span data-stu-id="73207-361">nvarchar</span></span> |<span data-ttu-id="73207-362">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="73207-362">String, Char[]</span></span> |
| <span data-ttu-id="73207-363">skutečné</span><span class="sxs-lookup"><span data-stu-id="73207-363">real</span></span> |<span data-ttu-id="73207-364">Jeden</span><span class="sxs-lookup"><span data-stu-id="73207-364">Single</span></span> |
| <span data-ttu-id="73207-365">ROWVERSION</span><span class="sxs-lookup"><span data-stu-id="73207-365">rowversion</span></span> |<span data-ttu-id="73207-366">Byte]</span><span class="sxs-lookup"><span data-stu-id="73207-366">Byte[]</span></span> |
| <span data-ttu-id="73207-367">smalldatetime</span><span class="sxs-lookup"><span data-stu-id="73207-367">smalldatetime</span></span> |<span data-ttu-id="73207-368">Data a času</span><span class="sxs-lookup"><span data-stu-id="73207-368">DateTime</span></span> |
| <span data-ttu-id="73207-369">smallint</span><span class="sxs-lookup"><span data-stu-id="73207-369">smallint</span></span> |<span data-ttu-id="73207-370">Int16</span><span class="sxs-lookup"><span data-stu-id="73207-370">Int16</span></span> |
| <span data-ttu-id="73207-371">Smallmoney</span><span class="sxs-lookup"><span data-stu-id="73207-371">smallmoney</span></span> |<span data-ttu-id="73207-372">Decimal</span><span class="sxs-lookup"><span data-stu-id="73207-372">Decimal</span></span> |
| <span data-ttu-id="73207-373">SQL_VARIANT</span><span class="sxs-lookup"><span data-stu-id="73207-373">sql_variant</span></span> |<span data-ttu-id="73207-374">Objekt *</span><span class="sxs-lookup"><span data-stu-id="73207-374">Object *</span></span> |
| <span data-ttu-id="73207-375">Text</span><span class="sxs-lookup"><span data-stu-id="73207-375">text</span></span> |<span data-ttu-id="73207-376">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="73207-376">String, Char[]</span></span> |
| <span data-ttu-id="73207-377">time</span><span class="sxs-lookup"><span data-stu-id="73207-377">time</span></span> |<span data-ttu-id="73207-378">Časový interval</span><span class="sxs-lookup"><span data-stu-id="73207-378">TimeSpan</span></span> |
| <span data-ttu-id="73207-379">časové razítko</span><span class="sxs-lookup"><span data-stu-id="73207-379">timestamp</span></span> |<span data-ttu-id="73207-380">Byte]</span><span class="sxs-lookup"><span data-stu-id="73207-380">Byte[]</span></span> |
| <span data-ttu-id="73207-381">tinyint</span><span class="sxs-lookup"><span data-stu-id="73207-381">tinyint</span></span> |<span data-ttu-id="73207-382">Bajtů</span><span class="sxs-lookup"><span data-stu-id="73207-382">Byte</span></span> |
| <span data-ttu-id="73207-383">Typ UniqueIdentifier</span><span class="sxs-lookup"><span data-stu-id="73207-383">uniqueidentifier</span></span> |<span data-ttu-id="73207-384">Identifikátor GUID</span><span class="sxs-lookup"><span data-stu-id="73207-384">Guid</span></span> |
| <span data-ttu-id="73207-385">varbinary</span><span class="sxs-lookup"><span data-stu-id="73207-385">varbinary</span></span> |<span data-ttu-id="73207-386">Byte]</span><span class="sxs-lookup"><span data-stu-id="73207-386">Byte[]</span></span> |
| <span data-ttu-id="73207-387">varchar</span><span class="sxs-lookup"><span data-stu-id="73207-387">varchar</span></span> |<span data-ttu-id="73207-388">Řetězec, Char]</span><span class="sxs-lookup"><span data-stu-id="73207-388">String, Char[]</span></span> |
| <span data-ttu-id="73207-389">xml</span><span class="sxs-lookup"><span data-stu-id="73207-389">xml</span></span> |<span data-ttu-id="73207-390">XML</span><span class="sxs-lookup"><span data-stu-id="73207-390">Xml</span></span> |

## <a name="map-source-toosink-columns"></a><span data-ttu-id="73207-391">Mapování zdrojových toosink sloupců</span><span class="sxs-lookup"><span data-stu-id="73207-391">Map source toosink columns</span></span>
<span data-ttu-id="73207-392">toolearn o mapování sloupců v toocolumns datové sady zdroje v datové sadě jímka, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="73207-392">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-copy"></a><span data-ttu-id="73207-393">Opakovatelných kopie</span><span class="sxs-lookup"><span data-stu-id="73207-393">Repeatable copy</span></span>
<span data-ttu-id="73207-394">Při kopírování dat tooSQL databáze serveru, aktivity kopírování hello připojí tabulky jímky toohello data ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="73207-394">When copying data tooSQL Server Database, hello copy activity appends data toohello sink table by default.</span></span> <span data-ttu-id="73207-395">Místo toho najdete v části tooperform UPSERT [tooSqlSink opakovatelných zápisu](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) článku.</span><span class="sxs-lookup"><span data-stu-id="73207-395">tooperform an UPSERT instead,  See [Repeatable write tooSqlSink](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) article.</span></span> 

<span data-ttu-id="73207-396">Při kopírování dat z relační datové úložiště, mějte opakovatelnosti pamatovat tooavoid nezamýšleným výstupy.</span><span class="sxs-lookup"><span data-stu-id="73207-396">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="73207-397">V Azure Data Factory může řez znovu ručně.</span><span class="sxs-lookup"><span data-stu-id="73207-397">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="73207-398">Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="73207-398">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="73207-399">Pokud v obou případech se znovu spustí řez, je potřeba toomake jisti, který hello stejných dat je pro čtení bez ohledu na to jak mnohokrát řez je spustit.</span><span class="sxs-lookup"><span data-stu-id="73207-399">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="73207-400">V tématu [Repeatable číst z relačními zdroji](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="73207-400">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="73207-401">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="73207-401">Performance and Tuning</span></span>
<span data-ttu-id="73207-402">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.</span><span class="sxs-lookup"><span data-stu-id="73207-402">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
