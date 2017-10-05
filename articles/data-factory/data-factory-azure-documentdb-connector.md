---
title: "Přesun dat do/z Azure Cosmos DB | Microsoft Docs"
description: "Zjistěte, jak přesunout data do nebo z kolekce Azure Cosmos DB pomocí Azure Data Factory"
services: data-factory, cosmosdb
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: c9297b71-1bb4-4b29-ba3c-4cf1f5575fac
ms.service: multiple
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: 7a11c6ade0325b08ad520448bbf82d64a0a555f3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="move-data-to-and-from-azure-cosmos-db-using-azure-data-factory"></a><span data-ttu-id="f3b23-103">Přesun dat do a z databáze Cosmos Azure pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="f3b23-103">Move data to and from Azure Cosmos DB using Azure Data Factory</span></span>
<span data-ttu-id="f3b23-104">Tento článek vysvětluje, jak pomocí aktivity kopírování v Azure Data Factory pro přesun dat z Azure DB Cosmos (DocumentDB rozhraní API).</span><span class="sxs-lookup"><span data-stu-id="f3b23-104">This article explains how to use the Copy Activity in Azure Data Factory to move data to/from Azure Cosmos DB (DocumentDB API).</span></span> <span data-ttu-id="f3b23-105">Vychází [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="f3b23-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span> 

<span data-ttu-id="f3b23-106">Data můžete zkopírovat z úložiště dat žádné podporované zdrojové pro Azure Cosmos DB nebo z Azure DB Cosmos do úložiště dat žádné podporované jímky.</span><span class="sxs-lookup"><span data-stu-id="f3b23-106">You can copy data from any supported source data store to Azure Cosmos DB or from Azure Cosmos DB to any supported sink data store.</span></span> <span data-ttu-id="f3b23-107">Seznam úložišť dat jako zdroje nebo jímky nepodporuje aktivitě kopírování najdete v tématu [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky.</span><span class="sxs-lookup"><span data-stu-id="f3b23-107">For a list of data stores supported as sources or sinks by the copy activity, see the [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="f3b23-108">Konektor Azure Cosmos DB podporují pouze DocumentDB API.</span><span class="sxs-lookup"><span data-stu-id="f3b23-108">Azure Cosmos DB connector only support DocumentDB API.</span></span>

<span data-ttu-id="f3b23-109">Ke zkopírování dat jako-je do nebo ze soubory JSON nebo jiné Cosmos DB kolekce najdete v části [dokumentů JSON importu a exportu](#importexport-json-documents).</span><span class="sxs-lookup"><span data-stu-id="f3b23-109">To copy data as-is to/from JSON files or another Cosmos DB collection, see [Import/Export JSON documents](#importexport-json-documents).</span></span>

## <a name="getting-started"></a><span data-ttu-id="f3b23-110">Začínáme</span><span class="sxs-lookup"><span data-stu-id="f3b23-110">Getting started</span></span>
<span data-ttu-id="f3b23-111">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z Azure Cosmos DB pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f3b23-111">You can create a pipeline with a copy activity that moves data to/from Azure Cosmos DB by using different tools/APIs.</span></span>

<span data-ttu-id="f3b23-112">Nejjednodušší způsob, jak vytvořit kanál je použití **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="f3b23-112">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="f3b23-113">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírováním data.</span><span class="sxs-lookup"><span data-stu-id="f3b23-113">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="f3b23-114">Tyto nástroje můžete také použít k vytvoření kanálu: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru**, **.NET API**, a **REST API**.</span><span class="sxs-lookup"><span data-stu-id="f3b23-114">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="f3b23-115">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="f3b23-115">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="f3b23-116">Jestli používáte nástroje nebo rozhraní API, je třeba provést následující kroky k vytvoření kanálu, který přesouvá data ze zdrojového úložiště dat do úložiště dat podřízený:</span><span class="sxs-lookup"><span data-stu-id="f3b23-116">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span> 

1. <span data-ttu-id="f3b23-117">Vytvoření **propojené služby** propojení vstupní a výstupní data ukládá do data factory.</span><span class="sxs-lookup"><span data-stu-id="f3b23-117">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="f3b23-118">Vytvoření **datové sady** představují vstupní a výstupní data pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="f3b23-118">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="f3b23-119">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="f3b23-119">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="f3b23-120">Když použijete průvodce, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál).</span><span class="sxs-lookup"><span data-stu-id="f3b23-120">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="f3b23-121">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), definujete tyto entity služby Data Factory pomocí formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="f3b23-121">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="f3b23-122">Ukázky s definicemi JSON entit služby Data Factory, které se používají ke zkopírování dat do nebo z databáze Cosmos naleznete v části [JSON příklady](#json-examples) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="f3b23-122">For samples with JSON definitions for Data Factory entities that are used to copy data to/from Cosmos DB, see [JSON examples](#json-examples) section of this article.</span></span> 

<span data-ttu-id="f3b23-123">Následující části obsahují podrobnosti o vlastnostech formátu JSON, které slouží k určení konkrétní entity služby Data Factory k databázi Cosmos:</span><span class="sxs-lookup"><span data-stu-id="f3b23-123">The following sections provide details about JSON properties that are used to define Data Factory entities specific to Cosmos DB:</span></span> 

## <a name="linked-service-properties"></a><span data-ttu-id="f3b23-124">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="f3b23-124">Linked service properties</span></span>
<span data-ttu-id="f3b23-125">Následující tabulka obsahuje popis JSON elementy, které jsou specifické pro Azure DB Cosmos propojené služby.</span><span class="sxs-lookup"><span data-stu-id="f3b23-125">The following table provides description for JSON elements specific to Azure Cosmos DB linked service.</span></span>

| <span data-ttu-id="f3b23-126">**Vlastnost**</span><span class="sxs-lookup"><span data-stu-id="f3b23-126">**Property**</span></span> | <span data-ttu-id="f3b23-127">**Popis**</span><span class="sxs-lookup"><span data-stu-id="f3b23-127">**Description**</span></span> | <span data-ttu-id="f3b23-128">**Požadované**</span><span class="sxs-lookup"><span data-stu-id="f3b23-128">**Required**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f3b23-129">type</span><span class="sxs-lookup"><span data-stu-id="f3b23-129">type</span></span> |<span data-ttu-id="f3b23-130">Vlastnost typu musí být nastavena na: **DocumentDb**</span><span class="sxs-lookup"><span data-stu-id="f3b23-130">The type property must be set to: **DocumentDb**</span></span> |<span data-ttu-id="f3b23-131">Ano</span><span class="sxs-lookup"><span data-stu-id="f3b23-131">Yes</span></span> |
| <span data-ttu-id="f3b23-132">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="f3b23-132">connectionString</span></span> |<span data-ttu-id="f3b23-133">Zadejte informace potřebné pro připojení k databázi Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f3b23-133">Specify information needed to connect to Azure Cosmos DB database.</span></span> |<span data-ttu-id="f3b23-134">Ano</span><span class="sxs-lookup"><span data-stu-id="f3b23-134">Yes</span></span> |

<span data-ttu-id="f3b23-135">Příklad:</span><span class="sxs-lookup"><span data-stu-id="f3b23-135">Example:</span></span>

```JSON
{
  "name": "CosmosDbLinkedService",
  "properties": {
    "type": "DocumentDb",
    "typeProperties": {
      "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
    }
  }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="f3b23-136">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="f3b23-136">Dataset properties</span></span>
<span data-ttu-id="f3b23-137">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datových sad naleznete [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="f3b23-137">For a full list of sections & properties available for defining datasets please refer to the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="f3b23-138">Oddíly jako struktura, dostupnosti a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="f3b23-138">Sections like structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="f3b23-139">V rámci typeProperties části se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění dat v úložišti.</span><span class="sxs-lookup"><span data-stu-id="f3b23-139">The typeProperties section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="f3b23-140">Rámci typeProperties část datové sady typ **DocumentDbCollection** má následující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="f3b23-140">The typeProperties section for the dataset of type **DocumentDbCollection** has the following properties.</span></span>

| <span data-ttu-id="f3b23-141">**Vlastnost**</span><span class="sxs-lookup"><span data-stu-id="f3b23-141">**Property**</span></span> | <span data-ttu-id="f3b23-142">**Popis**</span><span class="sxs-lookup"><span data-stu-id="f3b23-142">**Description**</span></span> | <span data-ttu-id="f3b23-143">**Požadované**</span><span class="sxs-lookup"><span data-stu-id="f3b23-143">**Required**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f3b23-144">Název_kolekce</span><span class="sxs-lookup"><span data-stu-id="f3b23-144">collectionName</span></span> |<span data-ttu-id="f3b23-145">Název kolekce dokumentů Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f3b23-145">Name of the Cosmos DB document collection.</span></span> |<span data-ttu-id="f3b23-146">Ano</span><span class="sxs-lookup"><span data-stu-id="f3b23-146">Yes</span></span> |

<span data-ttu-id="f3b23-147">Příklad:</span><span class="sxs-lookup"><span data-stu-id="f3b23-147">Example:</span></span>

```JSON
{
  "name": "PersonCosmosDbTable",
  "properties": {
    "type": "DocumentDbCollection",
    "linkedServiceName": "CosmosDbLinkedService",
    "typeProperties": {
      "collectionName": "Person"
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
### <a name="schema-by-data-factory"></a><span data-ttu-id="f3b23-148">Schéma službou Data Factory</span><span class="sxs-lookup"><span data-stu-id="f3b23-148">Schema by Data Factory</span></span>
<span data-ttu-id="f3b23-149">Služba Data Factory pro data bez schémat úložiště, jako je například Azure Cosmos DB, odvodí schéma v jednom z následujících způsobů:</span><span class="sxs-lookup"><span data-stu-id="f3b23-149">For schema-free data stores such as Azure Cosmos DB, the Data Factory service infers the schema in one of the following ways:</span></span>  

1. <span data-ttu-id="f3b23-150">Pokud zadáte strukturu dat pomocí **struktura** vlastnost v definici datové sady, služba Data Factory ctí tato struktura jako schéma.</span><span class="sxs-lookup"><span data-stu-id="f3b23-150">If you specify the structure of data by using the **structure** property in the dataset definition, the Data Factory service honors this structure as the schema.</span></span> <span data-ttu-id="f3b23-151">V takovém případě Pokud řádek neobsahuje hodnotu pro sloupec, bude poskytnuta hodnota null pro ni.</span><span class="sxs-lookup"><span data-stu-id="f3b23-151">In this case, if a row does not contain a value for a column, a null value will be provided for it.</span></span>
2. <span data-ttu-id="f3b23-152">Pokud nezadáte strukturu dat pomocí **struktura** vlastnost v definici datové sady, služba Data Factory odvodí schématu pomocí prvního řádku v datech.</span><span class="sxs-lookup"><span data-stu-id="f3b23-152">If you do not specify the structure of data by using the **structure** property in the dataset definition, the Data Factory service infers the schema by using the first row in the data.</span></span> <span data-ttu-id="f3b23-153">V takovém případě pokud první řádek neobsahuje úplnou schéma, některé sloupce budou chybět ve výsledku operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="f3b23-153">In this case, if the first row does not contain the full schema, some columns will be missing in the result of copy operation.</span></span>

<span data-ttu-id="f3b23-154">Osvědčeným postupem pro zdroje dat bez schémat, proto je zadat strukturu dat pomocí **struktura** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="f3b23-154">Therefore, for schema-free data sources, the best practice is to specify the structure of data using the **structure** property.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="f3b23-155">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="f3b23-155">Copy activity properties</span></span>
<span data-ttu-id="f3b23-156">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivity naleznete [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="f3b23-156">For a full list of sections & properties available for defining activities please refer to the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="f3b23-157">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="f3b23-157">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

> [!NOTE]
> <span data-ttu-id="f3b23-158">Aktivita kopírování přijímá pouze jeden vstup a vytváří jenom jeden výstup.</span><span class="sxs-lookup"><span data-stu-id="f3b23-158">The Copy Activity takes only one input and produces only one output.</span></span>

<span data-ttu-id="f3b23-159">Vlastnosti dostupné v rámci typeProperties části aktivity na druhé straně lišit každý typ aktivity a v případě aktivitě kopírování se liší v závislosti na typech zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="f3b23-159">Properties available in the typeProperties section of the activity on the other hand vary with each activity type and in case of Copy activity they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="f3b23-160">V případě aktivitu kopírování, pokud je zdroj typu **DocumentDbCollectionSource** následující vlastnosti jsou k dispozici v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="f3b23-160">In case of Copy activity when source is of type **DocumentDbCollectionSource** the following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="f3b23-161">**Vlastnost**</span><span class="sxs-lookup"><span data-stu-id="f3b23-161">**Property**</span></span> | <span data-ttu-id="f3b23-162">**Popis**</span><span class="sxs-lookup"><span data-stu-id="f3b23-162">**Description**</span></span> | <span data-ttu-id="f3b23-163">**Povolené hodnoty**</span><span class="sxs-lookup"><span data-stu-id="f3b23-163">**Allowed values**</span></span> | <span data-ttu-id="f3b23-164">**Požadované**</span><span class="sxs-lookup"><span data-stu-id="f3b23-164">**Required**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="f3b23-165">query</span><span class="sxs-lookup"><span data-stu-id="f3b23-165">query</span></span> |<span data-ttu-id="f3b23-166">Zadejte dotaz číst data.</span><span class="sxs-lookup"><span data-stu-id="f3b23-166">Specify the query to read data.</span></span> |<span data-ttu-id="f3b23-167">Řetězec nepodporuje Azure Cosmos DB dotazu.</span><span class="sxs-lookup"><span data-stu-id="f3b23-167">Query string supported by Azure Cosmos DB.</span></span> <br/><br/><span data-ttu-id="f3b23-168">Příklad:`SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"`</span><span class="sxs-lookup"><span data-stu-id="f3b23-168">Example: `SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"`</span></span> |<span data-ttu-id="f3b23-169">Ne</span><span class="sxs-lookup"><span data-stu-id="f3b23-169">No</span></span> <br/><br/><span data-ttu-id="f3b23-170">Pokud není zadaný příkaz jazyka SQL, který se spustí:`select <columns defined in structure> from mycollection`</span><span class="sxs-lookup"><span data-stu-id="f3b23-170">If not specified, the SQL statement that is executed: `select <columns defined in structure> from mycollection`</span></span> |
| <span data-ttu-id="f3b23-171">nestingSeparator</span><span class="sxs-lookup"><span data-stu-id="f3b23-171">nestingSeparator</span></span> |<span data-ttu-id="f3b23-172">Speciální znak indikující, že dokument je vnořený</span><span class="sxs-lookup"><span data-stu-id="f3b23-172">Special character to indicate that the document is nested</span></span> |<span data-ttu-id="f3b23-173">Libovolný znak.</span><span class="sxs-lookup"><span data-stu-id="f3b23-173">Any character.</span></span> <br/><br/><span data-ttu-id="f3b23-174">Azure Cosmos DB je úložiště typu NoSQL pro dokumenty JSON, kde jsou povoleny vnořené struktury.</span><span class="sxs-lookup"><span data-stu-id="f3b23-174">Azure Cosmos DB is a NoSQL store for JSON documents, where nested structures are allowed.</span></span> <span data-ttu-id="f3b23-175">Azure Data Factory umožňuje uživateli označují hierarchie prostřednictvím nestingSeparator, což je "."</span><span class="sxs-lookup"><span data-stu-id="f3b23-175">Azure Data Factory enables user to denote hierarchy via nestingSeparator, which is “.”</span></span> <span data-ttu-id="f3b23-176">v předchozích příkladech.</span><span class="sxs-lookup"><span data-stu-id="f3b23-176">in the above examples.</span></span> <span data-ttu-id="f3b23-177">S oddělovačem, aktivitě kopírování bude generovat objekt "Name" tři podřízené elementy nejprve, střední a příjmení podle "Name.First", "Name.Middle" a "Name.Last" v definici tabulky.</span><span class="sxs-lookup"><span data-stu-id="f3b23-177">With the separator, the copy activity will generate the “Name” object with three children elements First, Middle and Last, according to “Name.First”, “Name.Middle” and “Name.Last” in the table definition.</span></span> |<span data-ttu-id="f3b23-178">Ne</span><span class="sxs-lookup"><span data-stu-id="f3b23-178">No</span></span> |

<span data-ttu-id="f3b23-179">**DocumentDbCollectionSink** podporuje následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="f3b23-179">**DocumentDbCollectionSink** supports the following properties:</span></span>

| <span data-ttu-id="f3b23-180">**Vlastnost**</span><span class="sxs-lookup"><span data-stu-id="f3b23-180">**Property**</span></span> | <span data-ttu-id="f3b23-181">**Popis**</span><span class="sxs-lookup"><span data-stu-id="f3b23-181">**Description**</span></span> | <span data-ttu-id="f3b23-182">**Povolené hodnoty**</span><span class="sxs-lookup"><span data-stu-id="f3b23-182">**Allowed values**</span></span> | <span data-ttu-id="f3b23-183">**Požadované**</span><span class="sxs-lookup"><span data-stu-id="f3b23-183">**Required**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="f3b23-184">nestingSeparator</span><span class="sxs-lookup"><span data-stu-id="f3b23-184">nestingSeparator</span></span> |<span data-ttu-id="f3b23-185">Budete potřebovat speciální znak v názvu sloupce zdroj označíte, že vnořených dokumentů.</span><span class="sxs-lookup"><span data-stu-id="f3b23-185">A special character in the source column name to indicate that nested document is needed.</span></span> <br/><br/><span data-ttu-id="f3b23-186">Například výše: `Name.First` ve výstupu tabulky vytvoří následující strukturu JSON v dokumentu Cosmos DB:</span><span class="sxs-lookup"><span data-stu-id="f3b23-186">For example above: `Name.First` in the output table produces the following JSON structure in the Cosmos DB document:</span></span><br/><br/><span data-ttu-id="f3b23-187">"Název": {</span><span class="sxs-lookup"><span data-stu-id="f3b23-187">"Name": {</span></span><br/>    <span data-ttu-id="f3b23-188">"První": "Jan"</span><span class="sxs-lookup"><span data-stu-id="f3b23-188">"First": "John"</span></span><br/><span data-ttu-id="f3b23-189">},</span><span class="sxs-lookup"><span data-stu-id="f3b23-189">},</span></span> |<span data-ttu-id="f3b23-190">Znak, který se používá k oddělení úrovní vnoření.</span><span class="sxs-lookup"><span data-stu-id="f3b23-190">Character that is used to separate nesting levels.</span></span><br/><br/><span data-ttu-id="f3b23-191">Výchozí hodnota je `.` (tečka).</span><span class="sxs-lookup"><span data-stu-id="f3b23-191">Default value is `.` (dot).</span></span> |<span data-ttu-id="f3b23-192">Znak, který se používá k oddělení úrovní vnoření.</span><span class="sxs-lookup"><span data-stu-id="f3b23-192">Character that is used to separate nesting levels.</span></span> <br/><br/><span data-ttu-id="f3b23-193">Výchozí hodnota je `.` (tečka).</span><span class="sxs-lookup"><span data-stu-id="f3b23-193">Default value is `.` (dot).</span></span> |
| <span data-ttu-id="f3b23-194">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="f3b23-194">writeBatchSize</span></span> |<span data-ttu-id="f3b23-195">Počet paralelní požadavků do služby Azure Cosmos DB vytvářet dokumenty.</span><span class="sxs-lookup"><span data-stu-id="f3b23-195">Number of parallel requests to Azure Cosmos DB service to create documents.</span></span><br/><br/><span data-ttu-id="f3b23-196">Při kopírování dat z databáze Cosmos pomocí této vlastnosti lze optimalizovat výkon.</span><span class="sxs-lookup"><span data-stu-id="f3b23-196">You can fine-tune the performance when copying data to/from Cosmos DB by using this property.</span></span> <span data-ttu-id="f3b23-197">Lepšího výkonu můžete očekávat, když zvýšíte writeBatchSize, protože se odesílají další paralelní žádosti do databáze Cosmos.</span><span class="sxs-lookup"><span data-stu-id="f3b23-197">You can expect a better performance when you increase writeBatchSize because more parallel requests to Cosmos DB are sent.</span></span> <span data-ttu-id="f3b23-198">Ale budete muset vyhnout, omezení šířky pásma, který lze vyvolat chybovou zprávu: "Požadavků je velká".</span><span class="sxs-lookup"><span data-stu-id="f3b23-198">However you’ll need to avoid throttling that can throw the error message: "Request rate is large".</span></span><br/><br/><span data-ttu-id="f3b23-199">Omezení je určeno podle počtu faktorů, včetně velikosti dokumentů, počet podmínky v dokumentech, indexování zásad cílovou kolekci, atd. Pro operace kopírování, můžete použít kolekci lepší (např. S3) tak, aby měl nejvíce propustnost, které jsou k dispozici (2 500 žádostí jednotek za sekundu).</span><span class="sxs-lookup"><span data-stu-id="f3b23-199">Throttling is decided by a number of factors, including size of documents, number of terms in documents, indexing policy of target collection, etc. For copy operations, you can use a better collection (e.g. S3) to have the most throughput available (2,500 request units/second).</span></span> |<span data-ttu-id="f3b23-200">Integer</span><span class="sxs-lookup"><span data-stu-id="f3b23-200">Integer</span></span> |<span data-ttu-id="f3b23-201">Ne (výchozí: 5)</span><span class="sxs-lookup"><span data-stu-id="f3b23-201">No (default: 5)</span></span> |
| <span data-ttu-id="f3b23-202">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="f3b23-202">writeBatchTimeout</span></span> |<span data-ttu-id="f3b23-203">Počkejte, než čas na dokončení předtím, než vyprší časový limit operace.</span><span class="sxs-lookup"><span data-stu-id="f3b23-203">Wait time for the operation to complete before it times out.</span></span> |<span data-ttu-id="f3b23-204">Časový interval</span><span class="sxs-lookup"><span data-stu-id="f3b23-204">timespan</span></span><br/><br/> <span data-ttu-id="f3b23-205">Příklad: "00: 30:00" (30 minut).</span><span class="sxs-lookup"><span data-stu-id="f3b23-205">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="f3b23-206">Ne</span><span class="sxs-lookup"><span data-stu-id="f3b23-206">No</span></span> |

## <a name="importexport-json-documents"></a><span data-ttu-id="f3b23-207">Dokumenty JSON importu a exportu</span><span class="sxs-lookup"><span data-stu-id="f3b23-207">Import/Export JSON documents</span></span>
<span data-ttu-id="f3b23-208">Pomocí tohoto konektoru Cosmos DB, můžete snadno</span><span class="sxs-lookup"><span data-stu-id="f3b23-208">Using this Cosmos DB connector, you can easily</span></span>

* <span data-ttu-id="f3b23-209">Importujte dokumenty JSON z různých zdrojů do Cosmos databáze, včetně objektů Blob v Azure, Azure Data Lake, v místním systému souborů nebo jiných úložišť na základě souborů podporovaných službou Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="f3b23-209">Import JSON documents from various sources into Cosmos DB, including Azure Blob, Azure Data Lake, on-premises File System or other file-based stores supported by Azure Data Factory.</span></span>
* <span data-ttu-id="f3b23-210">Exportujte dokumenty JSON ze collecton Cosmos databáze do různých úložišť na základě souborů.</span><span class="sxs-lookup"><span data-stu-id="f3b23-210">Export JSON documents from Cosmos DB collecton into various file-based stores.</span></span>
* <span data-ttu-id="f3b23-211">Migrace dat mezi dvě kolekce Cosmos DB jako-je.</span><span class="sxs-lookup"><span data-stu-id="f3b23-211">Migrate data between two Cosmos DB collections as-is.</span></span>

<span data-ttu-id="f3b23-212">K dosažení taková kopie vázané na schéma</span><span class="sxs-lookup"><span data-stu-id="f3b23-212">To achieve such schema-agnostic copy,</span></span> 
* <span data-ttu-id="f3b23-213">Při použití Průvodce kopírováním, zkontrolujte **"exportovat jako-soubory JSON nebo kolekce Cosmos DB"** možnost.</span><span class="sxs-lookup"><span data-stu-id="f3b23-213">When using copy wizard, check the **"Export as-is to JSON files or Cosmos DB collection"** option.</span></span>
* <span data-ttu-id="f3b23-214">Když pomocí úpravy JSON, nezadávejte v části "struktura" v datových sad Cosmos DB ani vlastnost "nestingSeparator" na Cosmos DB zdroj/jímka v aktivitě kopírování.</span><span class="sxs-lookup"><span data-stu-id="f3b23-214">When using JSON editing, do not specify the "structure" section in Cosmos DB dataset(s) nor "nestingSeparator" property on Cosmos DB source/sink in copy activity.</span></span> <span data-ttu-id="f3b23-215">Importovat z / exportovat do formátu JSON souborů, v datové sadě úložiště souboru zadejte typ formátu jako "JsonFormat", "filePattern" Konfigurace a přeskočit nastavení formátu rest, najdete v části [formátu JSON](data-factory-supported-file-and-compression-formats.md#json-format) části na podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="f3b23-215">To import from/export to JSON files, in the file store dataset specify format type as "JsonFormat", config "filePattern" and skip the rest format settings, see [JSON format](data-factory-supported-file-and-compression-formats.md#json-format) section on details.</span></span>

## <a name="json-examples"></a><span data-ttu-id="f3b23-216">Příklady JSON</span><span class="sxs-lookup"><span data-stu-id="f3b23-216">JSON examples</span></span>
<span data-ttu-id="f3b23-217">Následující příklady poskytují ukázka JSON definice, které můžete použít k vytvoření kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="f3b23-217">The following examples provide sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="f3b23-218">Se ukazují, jak ke zkopírování dat do a z Azure Cosmos DB a Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="f3b23-218">They show how to copy data to and from Azure Cosmos DB and Azure Blob Storage.</span></span> <span data-ttu-id="f3b23-219">Nicméně je možné zkopírovat data **přímo** ze všech zdrojů do jakéhokoli z jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí aktivity kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="f3b23-219">However, data can be copied **directly** from any of the sources to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>

## <a name="example-copy-data-from-azure-cosmos-db-to-azure-blob"></a><span data-ttu-id="f3b23-220">Příklad: Kopírování dat z Azure Cosmos DB do objektu Blob Azure</span><span class="sxs-lookup"><span data-stu-id="f3b23-220">Example: Copy data from Azure Cosmos DB to Azure Blob</span></span>
<span data-ttu-id="f3b23-221">Znázorňuje následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="f3b23-221">The sample below shows:</span></span>

1. <span data-ttu-id="f3b23-222">Propojené služby typu [DocumentDb](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="f3b23-222">A linked service of type [DocumentDb](#linked-service-properties).</span></span>
2. <span data-ttu-id="f3b23-223">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="f3b23-223">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="f3b23-224">Vstup [datovou sadu](data-factory-create-datasets.md) typu [DocumentDbCollection](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="f3b23-224">An input [dataset](data-factory-create-datasets.md) of type [DocumentDbCollection](#dataset-properties).</span></span>
4. <span data-ttu-id="f3b23-225">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="f3b23-225">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="f3b23-226">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [DocumentDbCollectionSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="f3b23-226">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [DocumentDbCollectionSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="f3b23-227">Ukázka zkopíruje data v Azure Cosmos DB do objektu Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="f3b23-227">The sample copies data in Azure Cosmos DB to Azure Blob.</span></span> <span data-ttu-id="f3b23-228">Vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="f3b23-228">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="f3b23-229">**Azure Cosmos DB propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="f3b23-229">**Azure Cosmos DB linked service:**</span></span>

```JSON
{
  "name": "CosmosDbLinkedService",
  "properties": {
    "type": "DocumentDb",
    "typeProperties": {
      "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
    }
  }
}
```
<span data-ttu-id="f3b23-230">**Propojená služba Azure Blob storage:**</span><span class="sxs-lookup"><span data-stu-id="f3b23-230">**Azure Blob storage linked service:**</span></span>

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
<span data-ttu-id="f3b23-231">**Azure Documentdb vstupní datové sady:**</span><span class="sxs-lookup"><span data-stu-id="f3b23-231">**Azure Document DB input dataset:**</span></span>

<span data-ttu-id="f3b23-232">Příkladu se předpokládá, máte kolekci s názvem **osoba** v databázi Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f3b23-232">The sample assumes you have a collection named **Person** in an Azure Cosmos DB database.</span></span>

<span data-ttu-id="f3b23-233">Nastavení "externí": "PRAVDA" a zadání externalData informace o zásadách služby Azure Data Factory, v tabulce je externí k objektu pro vytváření dat a není vyprodukované aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="f3b23-233">Setting “external”: ”true” and specifying externalData policy information the Azure Data Factory service that the table is external to the data factory and not produced by an activity in the data factory.</span></span>

```JSON
{
  "name": "PersonCosmosDbTable",
  "properties": {
    "type": "DocumentDbCollection",
    "linkedServiceName": "CosmosDbLinkedService",
    "typeProperties": {
      "collectionName": "Person"
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

<span data-ttu-id="f3b23-234">**Azure Blob výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="f3b23-234">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="f3b23-235">Data se zkopírují do nového objektu blob každou hodinu s cestou pro tento objekt blob, které odráží konkrétní hodnotu datetime s rozlišením hodinu.</span><span class="sxs-lookup"><span data-stu-id="f3b23-235">Data is copied to a new blob every hour with the path for the blob reflecting the specific datetime with hour granularity.</span></span>

```JSON
{
  "name": "PersonBlobTableOut",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "docdb",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "nullValue": "NULL"
      }
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
<span data-ttu-id="f3b23-236">Ukázka dokumentu JSON v kolekci osoby v databázi Cosmos DB:</span><span class="sxs-lookup"><span data-stu-id="f3b23-236">Sample JSON document in the Person collection in a Cosmos DB database:</span></span>

```JSON
{
  "PersonId": 2,
  "Name": {
    "First": "Jane",
    "Middle": "",
    "Last": "Doe"
  }
}
```
<span data-ttu-id="f3b23-237">Cosmos DB podporuje dotazování dokumentů pomocí SQL jako syntaxe přes hierarchické dokumentů JSON.</span><span class="sxs-lookup"><span data-stu-id="f3b23-237">Cosmos DB supports querying documents using a SQL like syntax over hierarchical JSON documents.</span></span>

<span data-ttu-id="f3b23-238">Příklad:</span><span class="sxs-lookup"><span data-stu-id="f3b23-238">Example:</span></span> 

```sql
SELECT Person.PersonId, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person
```

<span data-ttu-id="f3b23-239">Následující kanál kopíruje data z kolekce osoby v Azure Cosmos DB databázi do objektu blob Azure.</span><span class="sxs-lookup"><span data-stu-id="f3b23-239">The following pipeline copies data from the Person collection in the Azure Cosmos DB database to an Azure blob.</span></span> <span data-ttu-id="f3b23-240">Jako součást aktivitě kopírování vstup a výstup nebyly zadány datové sady.</span><span class="sxs-lookup"><span data-stu-id="f3b23-240">As part of the copy activity the input and output datasets have been specified.</span></span>  

```JSON
{
  "name": "DocDbToBlobPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "DocumentDbCollectionSource",
            "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
            "nestingSeparator": "."
          },
          "sink": {
            "type": "BlobSink",
            "blobWriterAddHeader": true,
            "writeBatchSize": 1000,
            "writeBatchTimeout": "00:00:59"
          }
        },
        "inputs": [
          {
            "name": "PersonCosmosDbTable"
          }
        ],
        "outputs": [
          {
            "name": "PersonBlobTableOut"
          }
        ],
        "policy": {
          "concurrency": 1
        },
        "name": "CopyFromDocDbToBlob"
      }
    ],
    "start": "2015-04-01T00:00:00Z",
    "end": "2015-04-02T00:00:00Z"
  }
}
```
## <a name="example-copy-data-from-azure-blob-to-azure-cosmos-db"></a><span data-ttu-id="f3b23-241">Příklad: Kopírování dat z objektu Blob Azure do Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="f3b23-241">Example: Copy data from Azure Blob to Azure Cosmos DB</span></span> 
<span data-ttu-id="f3b23-242">Znázorňuje následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="f3b23-242">The sample below shows:</span></span>

1. <span data-ttu-id="f3b23-243">Propojené služby typu [DocumentDb](#azure-documentdb-linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="f3b23-243">A linked service of type [DocumentDb](#azure-documentdb-linked-service-properties).</span></span>
2. <span data-ttu-id="f3b23-244">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="f3b23-244">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="f3b23-245">Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="f3b23-245">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="f3b23-246">Výstup [datovou sadu](data-factory-create-datasets.md) typu [DocumentDbCollection](#azure-documentdb-dataset-type-properties).</span><span class="sxs-lookup"><span data-stu-id="f3b23-246">An output [dataset](data-factory-create-datasets.md) of type [DocumentDbCollection](#azure-documentdb-dataset-type-properties).</span></span>
5. <span data-ttu-id="f3b23-247">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) a [DocumentDbCollectionSink](#azure-documentdb-copy-activity-type-properties).</span><span class="sxs-lookup"><span data-stu-id="f3b23-247">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) and [DocumentDbCollectionSink](#azure-documentdb-copy-activity-type-properties).</span></span>

<span data-ttu-id="f3b23-248">Ukázka zkopíruje data z Azure blob Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f3b23-248">The sample copies data from Azure blob to Azure Cosmos DB.</span></span> <span data-ttu-id="f3b23-249">Vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="f3b23-249">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="f3b23-250">**Propojená služba Azure Blob storage:**</span><span class="sxs-lookup"><span data-stu-id="f3b23-250">**Azure Blob storage linked service:**</span></span>

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
<span data-ttu-id="f3b23-251">**Azure Cosmos DB propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="f3b23-251">**Azure Cosmos DB linked service:**</span></span>

```JSON
{
  "name": "CosmosDbLinkedService",
  "properties": {
    "type": "DocumentDb",
    "typeProperties": {
      "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
    }
  }
}
```
<span data-ttu-id="f3b23-252">**Azure vstupní datovou sadu objektu Blob:**</span><span class="sxs-lookup"><span data-stu-id="f3b23-252">**Azure Blob input dataset:**</span></span>

```JSON
{
  "name": "PersonBlobTableIn",
  "properties": {
    "structure": [
      {
        "name": "Id",
        "type": "Int"
      },
      {
        "name": "FirstName",
        "type": "String"
      },
      {
        "name": "MiddleName",
        "type": "String"
      },
      {
        "name": "LastName",
        "type": "String"
      }
    ],
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "fileName": "input.csv",
      "folderPath": "docdb",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "nullValue": "NULL"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
<span data-ttu-id="f3b23-253">**Azure Cosmos DB výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="f3b23-253">**Azure Cosmos DB output dataset:**</span></span>

<span data-ttu-id="f3b23-254">Ukázka zkopíruje data na kolekci s názvem "Osoba".</span><span class="sxs-lookup"><span data-stu-id="f3b23-254">The sample copies data to a collection named “Person”.</span></span>

```JSON
{
  "name": "PersonCosmosDbTableOut",
  "properties": {
    "structure": [
      {
        "name": "Id",
        "type": "Int"
      },
      {
        "name": "Name.First",
        "type": "String"
      },
      {
        "name": "Name.Middle",
        "type": "String"
      },
      {
        "name": "Name.Last",
        "type": "String"
      }
    ],
    "type": "DocumentDbCollection",
    "linkedServiceName": "CosmosDbLinkedService",
    "typeProperties": {
      "collectionName": "Person"
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
<span data-ttu-id="f3b23-255">Následující kanál kopíruje data z objektu Blob Azure do kolekce osoby v databázi Cosmos.</span><span class="sxs-lookup"><span data-stu-id="f3b23-255">The following pipeline copies data from Azure Blob to the Person collection in the Cosmos DB.</span></span> <span data-ttu-id="f3b23-256">Jako součást aktivitě kopírování vstup a výstup nebyly zadány datové sady.</span><span class="sxs-lookup"><span data-stu-id="f3b23-256">As part of the copy activity the input and output datasets have been specified.</span></span>

```JSON
{
  "name": "BlobToDocDbPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "DocumentDbCollectionSink",
            "nestingSeparator": ".",
            "writeBatchSize": 2,
            "writeBatchTimeout": "00:00:00"
          }
          "translator": {
              "type": "TabularTranslator",
              "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, Title: Title, Suffix: Suffix, EmailPromotion: EmailPromotion, rowguid: rowguid, ModifiedDate: ModifiedDate"
          }
        },
        "inputs": [
          {
            "name": "PersonBlobTableIn"
          }
        ],
        "outputs": [
          {
            "name": "PersonCosmosDbTableOut"
          }
        ],
        "policy": {
          "concurrency": 1
        },
        "name": "CopyFromBlobToDocDb"
      }
    ],
    "start": "2015-04-14T00:00:00Z",
    "end": "2015-04-15T00:00:00Z"
  }
}
```
<span data-ttu-id="f3b23-257">Pokud jako vstup objektu blob ukázka</span><span class="sxs-lookup"><span data-stu-id="f3b23-257">If the sample blob input is as</span></span>

```
1,John,,Doe
```
<span data-ttu-id="f3b23-258">Výstup JSON v Cosmos DB se pak bude jako:</span><span class="sxs-lookup"><span data-stu-id="f3b23-258">Then the output JSON in Cosmos DB will be as:</span></span>

```JSON
{
  "Id": 1,
  "Name": {
    "First": "John",
    "Middle": null,
    "Last": "Doe"
  },
  "id": "a5e8595c-62ec-4554-a118-3940f4ff70b6"
}
```
<span data-ttu-id="f3b23-259">Azure Cosmos DB je úložiště typu NoSQL pro dokumenty JSON, kde jsou povoleny vnořené struktury.</span><span class="sxs-lookup"><span data-stu-id="f3b23-259">Azure Cosmos DB is a NoSQL store for JSON documents, where nested structures are allowed.</span></span> <span data-ttu-id="f3b23-260">Azure Data Factory umožňuje uživateli označují hierarchie prostřednictvím **nestingSeparator**, což je "."</span><span class="sxs-lookup"><span data-stu-id="f3b23-260">Azure Data Factory enables user to denote hierarchy via **nestingSeparator**, which is “.”</span></span> <span data-ttu-id="f3b23-261">v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="f3b23-261">in this example.</span></span> <span data-ttu-id="f3b23-262">S oddělovačem, aktivitě kopírování bude generovat objekt "Name" tři podřízené elementy nejprve, střední a příjmení podle "Name.First", "Name.Middle" a "Name.Last" v definici tabulky.</span><span class="sxs-lookup"><span data-stu-id="f3b23-262">With the separator, the copy activity will generate the “Name” object with three children elements First, Middle and Last, according to “Name.First”, “Name.Middle” and “Name.Last” in the table definition.</span></span>

## <a name="appendix"></a><span data-ttu-id="f3b23-263">Příloha</span><span class="sxs-lookup"><span data-stu-id="f3b23-263">Appendix</span></span>
1. <span data-ttu-id="f3b23-264">**Otázka:** nemá aktualizace podporu aktivity kopírování existující záznamy?</span><span class="sxs-lookup"><span data-stu-id="f3b23-264">**Question:** Does the Copy Activity support update of existing records?</span></span>

    <span data-ttu-id="f3b23-265">**Odpověď:** ne.</span><span class="sxs-lookup"><span data-stu-id="f3b23-265">**Answer:** No.</span></span>
2. <span data-ttu-id="f3b23-266">**Otázka:** jak již nemá opakování kopii Azure Cosmos DB věcech zkopírovat záznamy?</span><span class="sxs-lookup"><span data-stu-id="f3b23-266">**Question:** How does a retry of a copy to Azure Cosmos DB deal with already copied records?</span></span>

    <span data-ttu-id="f3b23-267">**Odpověď:** Pokud záznamy na pole "ID" a kopírování pokusí vložit záznam se stejným ID, operace kopírování vrátí chybu.</span><span class="sxs-lookup"><span data-stu-id="f3b23-267">**Answer:** If records have an "ID" field and the copy operation tries to insert a record with the same ID, the copy operation throws an error.</span></span>  
3. <span data-ttu-id="f3b23-268">**Otázka:** podporuje služby Data Factory [rozsah nebo dělení dat na základě hodnoty hash](../documentdb/documentdb-partition-data.md)?</span><span class="sxs-lookup"><span data-stu-id="f3b23-268">**Question:** Does Data Factory support [range or hash-based data partitioning](../documentdb/documentdb-partition-data.md)?</span></span>

    <span data-ttu-id="f3b23-269">**Odpověď:** ne.</span><span class="sxs-lookup"><span data-stu-id="f3b23-269">**Answer:** No.</span></span>
4. <span data-ttu-id="f3b23-270">**Otázka:** můžete zadat více než jednu kolekci Azure Cosmos DB pro tabulku?</span><span class="sxs-lookup"><span data-stu-id="f3b23-270">**Question:** Can I specify more than one Azure Cosmos DB collection for a table?</span></span>

    <span data-ttu-id="f3b23-271">**Odpověď:** ne.</span><span class="sxs-lookup"><span data-stu-id="f3b23-271">**Answer:** No.</span></span> <span data-ttu-id="f3b23-272">V tuto chvíli je možné zadat pouze jednu kolekci.</span><span class="sxs-lookup"><span data-stu-id="f3b23-272">Only one collection can be specified at this time.</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="f3b23-273">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="f3b23-273">Performance and Tuning</span></span>
<span data-ttu-id="f3b23-274">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) Další informace o klíčových faktorů, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby, jak optimalizovat ho.</span><span class="sxs-lookup"><span data-stu-id="f3b23-274">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
