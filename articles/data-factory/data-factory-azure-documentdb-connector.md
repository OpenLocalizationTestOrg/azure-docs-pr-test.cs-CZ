---
title: aaaMove data do/z Azure Cosmos DB | Microsoft Docs
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
ms.openlocfilehash: bd23ce4e004a972ce6f3e4165cfdea4f0c18fecc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooand-from-azure-cosmos-db-using-azure-data-factory"></a><span data-ttu-id="1d3f5-103">Přesunout data tooand z databáze Cosmos Azure pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="1d3f5-103">Move data tooand from Azure Cosmos DB using Azure Data Factory</span></span>
<span data-ttu-id="1d3f5-104">Tento článek vysvětluje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove dat z Azure DB Cosmos (DocumentDB rozhraní API).</span><span class="sxs-lookup"><span data-stu-id="1d3f5-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data to/from Azure Cosmos DB (DocumentDB API).</span></span> <span data-ttu-id="1d3f5-105">Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span> 

<span data-ttu-id="1d3f5-106">Data můžete zkopírovat z jakéhokoli podporované zdroje dat ukládání tooAzure Cosmos DB nebo z Azure Cosmos DB tooany podporované jímku dat úložiště.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-106">You can copy data from any supported source data store tooAzure Cosmos DB or from Azure Cosmos DB tooany supported sink data store.</span></span> <span data-ttu-id="1d3f5-107">Seznam úložišť dat jako zdroje nebo jímky podporované aktivitou kopírování hello najdete v tématu hello [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-107">For a list of data stores supported as sources or sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="1d3f5-108">Konektor Azure Cosmos DB podporují pouze DocumentDB API.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-108">Azure Cosmos DB connector only support DocumentDB API.</span></span>

<span data-ttu-id="1d3f5-109">data toocopy jako-je do nebo ze soubory JSON nebo jiné Cosmos DB kolekce najdete v části [dokumentů JSON importu a exportu](#importexport-json-documents).</span><span class="sxs-lookup"><span data-stu-id="1d3f5-109">toocopy data as-is to/from JSON files or another Cosmos DB collection, see [Import/Export JSON documents](#importexport-json-documents).</span></span>

## <a name="getting-started"></a><span data-ttu-id="1d3f5-110">Začínáme</span><span class="sxs-lookup"><span data-stu-id="1d3f5-110">Getting started</span></span>
<span data-ttu-id="1d3f5-111">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z Azure Cosmos DB pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-111">You can create a pipeline with a copy activity that moves data to/from Azure Cosmos DB by using different tools/APIs.</span></span>

<span data-ttu-id="1d3f5-112">Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-112">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="1d3f5-113">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-113">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="1d3f5-114">Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-114">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="1d3f5-115">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-115">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="1d3f5-116">Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:</span><span class="sxs-lookup"><span data-stu-id="1d3f5-116">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span> 

1. <span data-ttu-id="1d3f5-117">Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-117">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="1d3f5-118">Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-118">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="1d3f5-119">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-119">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="1d3f5-120">Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello).</span><span class="sxs-lookup"><span data-stu-id="1d3f5-120">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="1d3f5-121">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-121">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="1d3f5-122">Ukázky s definicemi JSON entit služby Data Factory, které jsou používané toocopy data do nebo z databáze Cosmos naleznete v části [JSON příklady](#json-examples) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-122">For samples with JSON definitions for Data Factory entities that are used toocopy data to/from Cosmos DB, see [JSON examples](#json-examples) section of this article.</span></span> 

<span data-ttu-id="1d3f5-123">Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooCosmos DB:</span><span class="sxs-lookup"><span data-stu-id="1d3f5-123">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooCosmos DB:</span></span> 

## <a name="linked-service-properties"></a><span data-ttu-id="1d3f5-124">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="1d3f5-124">Linked service properties</span></span>
<span data-ttu-id="1d3f5-125">Hello následující tabulka obsahuje popis pro konkrétní tooAzure elementy JSON Cosmos DB propojené služby.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-125">hello following table provides description for JSON elements specific tooAzure Cosmos DB linked service.</span></span>

| <span data-ttu-id="1d3f5-126">**Vlastnost**</span><span class="sxs-lookup"><span data-stu-id="1d3f5-126">**Property**</span></span> | <span data-ttu-id="1d3f5-127">**Popis**</span><span class="sxs-lookup"><span data-stu-id="1d3f5-127">**Description**</span></span> | <span data-ttu-id="1d3f5-128">**Požadované**</span><span class="sxs-lookup"><span data-stu-id="1d3f5-128">**Required**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1d3f5-129">type</span><span class="sxs-lookup"><span data-stu-id="1d3f5-129">type</span></span> |<span data-ttu-id="1d3f5-130">vlastnost typu Hello musí být nastavena na: **DocumentDb**</span><span class="sxs-lookup"><span data-stu-id="1d3f5-130">hello type property must be set to: **DocumentDb**</span></span> |<span data-ttu-id="1d3f5-131">Ano</span><span class="sxs-lookup"><span data-stu-id="1d3f5-131">Yes</span></span> |
| <span data-ttu-id="1d3f5-132">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="1d3f5-132">connectionString</span></span> |<span data-ttu-id="1d3f5-133">Zadejte informace potřebné tooconnect tooAzure Cosmos DB databáze.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-133">Specify information needed tooconnect tooAzure Cosmos DB database.</span></span> |<span data-ttu-id="1d3f5-134">Ano</span><span class="sxs-lookup"><span data-stu-id="1d3f5-134">Yes</span></span> |

<span data-ttu-id="1d3f5-135">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1d3f5-135">Example:</span></span>

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

## <a name="dataset-properties"></a><span data-ttu-id="1d3f5-136">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="1d3f5-136">Dataset properties</span></span>
<span data-ttu-id="1d3f5-137">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datových sad naleznete toohello [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-137">For a full list of sections & properties available for defining datasets please refer toohello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="1d3f5-138">Oddíly jako struktura, dostupnosti a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="1d3f5-138">Sections like structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="1d3f5-139">část rámci typeProperties Hello se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-139">hello typeProperties section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="1d3f5-140">rámci typeProperties Hello části pro datovou sadu hello typu **DocumentDbCollection** má následující vlastnosti hello.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-140">hello typeProperties section for hello dataset of type **DocumentDbCollection** has hello following properties.</span></span>

| <span data-ttu-id="1d3f5-141">**Vlastnost**</span><span class="sxs-lookup"><span data-stu-id="1d3f5-141">**Property**</span></span> | <span data-ttu-id="1d3f5-142">**Popis**</span><span class="sxs-lookup"><span data-stu-id="1d3f5-142">**Description**</span></span> | <span data-ttu-id="1d3f5-143">**Požadované**</span><span class="sxs-lookup"><span data-stu-id="1d3f5-143">**Required**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1d3f5-144">Název_kolekce</span><span class="sxs-lookup"><span data-stu-id="1d3f5-144">collectionName</span></span> |<span data-ttu-id="1d3f5-145">Název hello kolekce dokumentů Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-145">Name of hello Cosmos DB document collection.</span></span> |<span data-ttu-id="1d3f5-146">Ano</span><span class="sxs-lookup"><span data-stu-id="1d3f5-146">Yes</span></span> |

<span data-ttu-id="1d3f5-147">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1d3f5-147">Example:</span></span>

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
### <a name="schema-by-data-factory"></a><span data-ttu-id="1d3f5-148">Schéma službou Data Factory</span><span class="sxs-lookup"><span data-stu-id="1d3f5-148">Schema by Data Factory</span></span>
<span data-ttu-id="1d3f5-149">Pro data bez schémat úložiště, jako je například Azure Cosmos DB odvodí hello služba Data Factory hello schéma v jednom z následujících způsobů hello:</span><span class="sxs-lookup"><span data-stu-id="1d3f5-149">For schema-free data stores such as Azure Cosmos DB, hello Data Factory service infers hello schema in one of hello following ways:</span></span>  

1. <span data-ttu-id="1d3f5-150">Pokud zadáte hello strukturu dat pomocí hello **struktura** vlastnost v definici datové sady hello, hello služba Data Factory ctí tato struktura jako hello schématu.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-150">If you specify hello structure of data by using hello **structure** property in hello dataset definition, hello Data Factory service honors this structure as hello schema.</span></span> <span data-ttu-id="1d3f5-151">V takovém případě Pokud řádek neobsahuje hodnotu pro sloupec, bude poskytnuta hodnota null pro ni.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-151">In this case, if a row does not contain a value for a column, a null value will be provided for it.</span></span>
2. <span data-ttu-id="1d3f5-152">Pokud nezadáte hello strukturu dat pomocí hello **struktura** vlastnost v definici datové sady hello, hello služba Data Factory odvodí hello schématu pomocí hello první řádek v datech hello.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-152">If you do not specify hello structure of data by using hello **structure** property in hello dataset definition, hello Data Factory service infers hello schema by using hello first row in hello data.</span></span> <span data-ttu-id="1d3f5-153">V takovém případě pokud první řádek hello neobsahuje úplnou schématu hello, některé sloupce budou chybět v hello výsledek operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-153">In this case, if hello first row does not contain hello full schema, some columns will be missing in hello result of copy operation.</span></span>

<span data-ttu-id="1d3f5-154">Proto pro zdroje dat bez schémat, hello osvědčeným postupem je toospecify hello strukturu dat pomocí hello **struktura** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-154">Therefore, for schema-free data sources, hello best practice is toospecify hello structure of data using hello **structure** property.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="1d3f5-155">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="1d3f5-155">Copy activity properties</span></span>
<span data-ttu-id="1d3f5-156">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivity naleznete toohello [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-156">For a full list of sections & properties available for defining activities please refer toohello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="1d3f5-157">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-157">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

> [!NOTE]
> <span data-ttu-id="1d3f5-158">Hello aktivity kopírování přijímá pouze jeden vstup a vytváří jenom jeden výstup.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-158">hello Copy Activity takes only one input and produces only one output.</span></span>

<span data-ttu-id="1d3f5-159">Vlastnosti dostupné v rámci typeProperties části hello hello aktivit na hello lišit druhé straně, každý typ aktivity a v případě aktivitě kopírování se liší v závislosti na typech hello zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-159">Properties available in hello typeProperties section of hello activity on hello other hand vary with each activity type and in case of Copy activity they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="1d3f5-160">V případě aktivitu kopírování, pokud je zdroj typu **DocumentDbCollectionSource** hello následující vlastnosti jsou k dispozici v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="1d3f5-160">In case of Copy activity when source is of type **DocumentDbCollectionSource** hello following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="1d3f5-161">**Vlastnost**</span><span class="sxs-lookup"><span data-stu-id="1d3f5-161">**Property**</span></span> | <span data-ttu-id="1d3f5-162">**Popis**</span><span class="sxs-lookup"><span data-stu-id="1d3f5-162">**Description**</span></span> | <span data-ttu-id="1d3f5-163">**Povolené hodnoty**</span><span class="sxs-lookup"><span data-stu-id="1d3f5-163">**Allowed values**</span></span> | <span data-ttu-id="1d3f5-164">**Požadované**</span><span class="sxs-lookup"><span data-stu-id="1d3f5-164">**Required**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="1d3f5-165">query</span><span class="sxs-lookup"><span data-stu-id="1d3f5-165">query</span></span> |<span data-ttu-id="1d3f5-166">Zadejte hello dotazu tooread data.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-166">Specify hello query tooread data.</span></span> |<span data-ttu-id="1d3f5-167">Řetězec nepodporuje Azure Cosmos DB dotazu.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-167">Query string supported by Azure Cosmos DB.</span></span> <br/><br/><span data-ttu-id="1d3f5-168">Příklad:`SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"`</span><span class="sxs-lookup"><span data-stu-id="1d3f5-168">Example: `SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"`</span></span> |<span data-ttu-id="1d3f5-169">Ne</span><span class="sxs-lookup"><span data-stu-id="1d3f5-169">No</span></span> <br/><br/><span data-ttu-id="1d3f5-170">Pokud není zadaný, hello příkaz jazyka SQL, který se spustí:`select <columns defined in structure> from mycollection`</span><span class="sxs-lookup"><span data-stu-id="1d3f5-170">If not specified, hello SQL statement that is executed: `select <columns defined in structure> from mycollection`</span></span> |
| <span data-ttu-id="1d3f5-171">nestingSeparator</span><span class="sxs-lookup"><span data-stu-id="1d3f5-171">nestingSeparator</span></span> |<span data-ttu-id="1d3f5-172">Je vnořený tooindicate speciální znak, který hello dokumentu</span><span class="sxs-lookup"><span data-stu-id="1d3f5-172">Special character tooindicate that hello document is nested</span></span> |<span data-ttu-id="1d3f5-173">Libovolný znak.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-173">Any character.</span></span> <br/><br/><span data-ttu-id="1d3f5-174">Azure Cosmos DB je úložiště typu NoSQL pro dokumenty JSON, kde jsou povoleny vnořené struktury.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-174">Azure Cosmos DB is a NoSQL store for JSON documents, where nested structures are allowed.</span></span> <span data-ttu-id="1d3f5-175">Azure Data Factory umožňuje hierarchie toodenote uživatele prostřednictvím nestingSeparator, což je "."</span><span class="sxs-lookup"><span data-stu-id="1d3f5-175">Azure Data Factory enables user toodenote hierarchy via nestingSeparator, which is “.”</span></span> <span data-ttu-id="1d3f5-176">v hello výše příklady.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-176">in hello above examples.</span></span> <span data-ttu-id="1d3f5-177">S oddělovačem hello aktivity kopírování hello vygeneruje hello "Name" objekt s tří podřízených elementů první, střední a poslední, podle too"Name.First", "Name.Middle" a "Name.Last" v hello Definice tabulky.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-177">With hello separator, hello copy activity will generate hello “Name” object with three children elements First, Middle and Last, according too“Name.First”, “Name.Middle” and “Name.Last” in hello table definition.</span></span> |<span data-ttu-id="1d3f5-178">Ne</span><span class="sxs-lookup"><span data-stu-id="1d3f5-178">No</span></span> |

<span data-ttu-id="1d3f5-179">**DocumentDbCollectionSink** podporuje hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="1d3f5-179">**DocumentDbCollectionSink** supports hello following properties:</span></span>

| <span data-ttu-id="1d3f5-180">**Vlastnost**</span><span class="sxs-lookup"><span data-stu-id="1d3f5-180">**Property**</span></span> | <span data-ttu-id="1d3f5-181">**Popis**</span><span class="sxs-lookup"><span data-stu-id="1d3f5-181">**Description**</span></span> | <span data-ttu-id="1d3f5-182">**Povolené hodnoty**</span><span class="sxs-lookup"><span data-stu-id="1d3f5-182">**Allowed values**</span></span> | <span data-ttu-id="1d3f5-183">**Požadované**</span><span class="sxs-lookup"><span data-stu-id="1d3f5-183">**Required**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="1d3f5-184">nestingSeparator</span><span class="sxs-lookup"><span data-stu-id="1d3f5-184">nestingSeparator</span></span> |<span data-ttu-id="1d3f5-185">Je potřeba speciálního znaku v hello zdrojový sloupec název tooindicate, který vnořených dokumentů.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-185">A special character in hello source column name tooindicate that nested document is needed.</span></span> <br/><br/><span data-ttu-id="1d3f5-186">Například výše: `Name.First` ve výstupu hello tabulky vytváří hello strukturu JSON v dokumentu Cosmos DB hello:</span><span class="sxs-lookup"><span data-stu-id="1d3f5-186">For example above: `Name.First` in hello output table produces hello following JSON structure in hello Cosmos DB document:</span></span><br/><br/><span data-ttu-id="1d3f5-187">"Název": {</span><span class="sxs-lookup"><span data-stu-id="1d3f5-187">"Name": {</span></span><br/>    <span data-ttu-id="1d3f5-188">"První": "Jan"</span><span class="sxs-lookup"><span data-stu-id="1d3f5-188">"First": "John"</span></span><br/><span data-ttu-id="1d3f5-189">},</span><span class="sxs-lookup"><span data-stu-id="1d3f5-189">},</span></span> |<span data-ttu-id="1d3f5-190">Znak, který je použité tooseparate vnořených úrovní.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-190">Character that is used tooseparate nesting levels.</span></span><br/><br/><span data-ttu-id="1d3f5-191">Výchozí hodnota je `.` (tečka).</span><span class="sxs-lookup"><span data-stu-id="1d3f5-191">Default value is `.` (dot).</span></span> |<span data-ttu-id="1d3f5-192">Znak, který je použité tooseparate vnořených úrovní.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-192">Character that is used tooseparate nesting levels.</span></span> <br/><br/><span data-ttu-id="1d3f5-193">Výchozí hodnota je `.` (tečka).</span><span class="sxs-lookup"><span data-stu-id="1d3f5-193">Default value is `.` (dot).</span></span> |
| <span data-ttu-id="1d3f5-194">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="1d3f5-194">writeBatchSize</span></span> |<span data-ttu-id="1d3f5-195">Počet paralelní požadavků tooAzure Cosmos DB služby toocreate dokumenty.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-195">Number of parallel requests tooAzure Cosmos DB service toocreate documents.</span></span><br/><br/><span data-ttu-id="1d3f5-196">Při kopírování dat z databáze Cosmos pomocí této vlastnosti lze optimalizovat výkon hello.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-196">You can fine-tune hello performance when copying data to/from Cosmos DB by using this property.</span></span> <span data-ttu-id="1d3f5-197">Lepšího výkonu můžete očekávat, když zvýšíte writeBatchSize, protože se odesílají další paralelní požadavky tooCosmos DB.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-197">You can expect a better performance when you increase writeBatchSize because more parallel requests tooCosmos DB are sent.</span></span> <span data-ttu-id="1d3f5-198">Ale budete potřebovat tooavoid omezení, který lze vyvolat hello chybová zpráva: "Požadavků je velká".</span><span class="sxs-lookup"><span data-stu-id="1d3f5-198">However you’ll need tooavoid throttling that can throw hello error message: "Request rate is large".</span></span><br/><br/><span data-ttu-id="1d3f5-199">Omezení je určeno podle počtu faktorů, včetně velikosti dokumentů, počet podmínky v dokumentech, indexování zásad cílovou kolekci, atd. Pro operace kopírování, můžete použít lepší hello toohave kolekce (např. S3) většina propustnost, které jsou k dispozici (2 500 žádostí jednotek za sekundu).</span><span class="sxs-lookup"><span data-stu-id="1d3f5-199">Throttling is decided by a number of factors, including size of documents, number of terms in documents, indexing policy of target collection, etc. For copy operations, you can use a better collection (e.g. S3) toohave hello most throughput available (2,500 request units/second).</span></span> |<span data-ttu-id="1d3f5-200">Integer</span><span class="sxs-lookup"><span data-stu-id="1d3f5-200">Integer</span></span> |<span data-ttu-id="1d3f5-201">Ne (výchozí: 5)</span><span class="sxs-lookup"><span data-stu-id="1d3f5-201">No (default: 5)</span></span> |
| <span data-ttu-id="1d3f5-202">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="1d3f5-202">writeBatchTimeout</span></span> |<span data-ttu-id="1d3f5-203">Doba pro operaci toocomplete hello Počkejte, než vyprší časový limit.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-203">Wait time for hello operation toocomplete before it times out.</span></span> |<span data-ttu-id="1d3f5-204">Časový interval</span><span class="sxs-lookup"><span data-stu-id="1d3f5-204">timespan</span></span><br/><br/> <span data-ttu-id="1d3f5-205">Příklad: "00: 30:00" (30 minut).</span><span class="sxs-lookup"><span data-stu-id="1d3f5-205">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="1d3f5-206">Ne</span><span class="sxs-lookup"><span data-stu-id="1d3f5-206">No</span></span> |

## <a name="importexport-json-documents"></a><span data-ttu-id="1d3f5-207">Dokumenty JSON importu a exportu</span><span class="sxs-lookup"><span data-stu-id="1d3f5-207">Import/Export JSON documents</span></span>
<span data-ttu-id="1d3f5-208">Pomocí tohoto konektoru Cosmos DB, můžete snadno</span><span class="sxs-lookup"><span data-stu-id="1d3f5-208">Using this Cosmos DB connector, you can easily</span></span>

* <span data-ttu-id="1d3f5-209">Importujte dokumenty JSON z různých zdrojů do Cosmos databáze, včetně objektů Blob v Azure, Azure Data Lake, v místním systému souborů nebo jiných úložišť na základě souborů podporovaných službou Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-209">Import JSON documents from various sources into Cosmos DB, including Azure Blob, Azure Data Lake, on-premises File System or other file-based stores supported by Azure Data Factory.</span></span>
* <span data-ttu-id="1d3f5-210">Exportujte dokumenty JSON ze collecton Cosmos databáze do různých úložišť na základě souborů.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-210">Export JSON documents from Cosmos DB collecton into various file-based stores.</span></span>
* <span data-ttu-id="1d3f5-211">Migrace dat mezi dvě kolekce Cosmos DB jako-je.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-211">Migrate data between two Cosmos DB collections as-is.</span></span>

<span data-ttu-id="1d3f5-212">tooachieve takové schématu bez ohledu na Kopírovat,</span><span class="sxs-lookup"><span data-stu-id="1d3f5-212">tooachieve such schema-agnostic copy,</span></span> 
* <span data-ttu-id="1d3f5-213">Při použití Průvodce kopírováním, zkontrolujte hello **"exportovat jako-tooJSON soubory nebo kolekce Cosmos DB"** možnost.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-213">When using copy wizard, check hello **"Export as-is tooJSON files or Cosmos DB collection"** option.</span></span>
* <span data-ttu-id="1d3f5-214">Když pomocí úpravy JSON, nezadávejte část "struktura" hello v datových sad Cosmos DB ani vlastnost "nestingSeparator" na Cosmos DB zdroj/jímka v aktivitě kopírování.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-214">When using JSON editing, do not specify hello "structure" section in Cosmos DB dataset(s) nor "nestingSeparator" property on Cosmos DB source/sink in copy activity.</span></span> <span data-ttu-id="1d3f5-215">tooimport z / exportovat soubory tooJSON, v datové sadě úložiště hello souboru zadejte typ formátu jako "JsonFormat", "filePattern" Konfigurace a přeskočit nastavení formátu hello rest, najdete v části [formátu JSON](data-factory-supported-file-and-compression-formats.md#json-format) části na podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-215">tooimport from/export tooJSON files, in hello file store dataset specify format type as "JsonFormat", config "filePattern" and skip hello rest format settings, see [JSON format](data-factory-supported-file-and-compression-formats.md#json-format) section on details.</span></span>

## <a name="json-examples"></a><span data-ttu-id="1d3f5-216">Příklady JSON</span><span class="sxs-lookup"><span data-stu-id="1d3f5-216">JSON examples</span></span>
<span data-ttu-id="1d3f5-217">Hello následující příklady poskytují definice JSON ukázka používané toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="1d3f5-217">hello following examples provide sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="1d3f5-218">Ukazují jak toocopy tooand data z databáze Cosmos Azure a Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-218">They show how toocopy data tooand from Azure Cosmos DB and Azure Blob Storage.</span></span> <span data-ttu-id="1d3f5-219">Však můžete zkopírovat data **přímo** z jakéhokoli zdroje tooany hello z hello jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-219">However, data can be copied **directly** from any of hello sources tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>

## <a name="example-copy-data-from-azure-cosmos-db-tooazure-blob"></a><span data-ttu-id="1d3f5-220">Příklad: Kopírování dat z Azure Cosmos DB tooAzure objektů Blob</span><span class="sxs-lookup"><span data-stu-id="1d3f5-220">Example: Copy data from Azure Cosmos DB tooAzure Blob</span></span>
<span data-ttu-id="1d3f5-221">znázorňuje následující ukázka Hello:</span><span class="sxs-lookup"><span data-stu-id="1d3f5-221">hello sample below shows:</span></span>

1. <span data-ttu-id="1d3f5-222">Propojené služby typu [DocumentDb](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="1d3f5-222">A linked service of type [DocumentDb](#linked-service-properties).</span></span>
2. <span data-ttu-id="1d3f5-223">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="1d3f5-223">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="1d3f5-224">Vstup [datovou sadu](data-factory-create-datasets.md) typu [DocumentDbCollection](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="1d3f5-224">An input [dataset](data-factory-create-datasets.md) of type [DocumentDbCollection](#dataset-properties).</span></span>
4. <span data-ttu-id="1d3f5-225">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="1d3f5-225">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="1d3f5-226">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [DocumentDbCollectionSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="1d3f5-226">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [DocumentDbCollectionSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="1d3f5-227">Ukázka Hello zkopíruje data v Azure Cosmos DB tooAzure objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-227">hello sample copies data in Azure Cosmos DB tooAzure Blob.</span></span> <span data-ttu-id="1d3f5-228">Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-228">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="1d3f5-229">**Azure Cosmos DB propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="1d3f5-229">**Azure Cosmos DB linked service:**</span></span>

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
<span data-ttu-id="1d3f5-230">**Propojená služba Azure Blob storage:**</span><span class="sxs-lookup"><span data-stu-id="1d3f5-230">**Azure Blob storage linked service:**</span></span>

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
<span data-ttu-id="1d3f5-231">**Azure Documentdb vstupní datové sady:**</span><span class="sxs-lookup"><span data-stu-id="1d3f5-231">**Azure Document DB input dataset:**</span></span>

<span data-ttu-id="1d3f5-232">Ukázka Hello předpokládá, že máte kolekci s názvem **osoba** v databázi Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-232">hello sample assumes you have a collection named **Person** in an Azure Cosmos DB database.</span></span>

<span data-ttu-id="1d3f5-233">Nastavení "externí": "PRAVDA" a zadání externalData informace o zásadách hello Azure Data Factory služby Tato tabulka hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-233">Setting “external”: ”true” and specifying externalData policy information hello Azure Data Factory service that hello table is external toohello data factory and not produced by an activity in hello data factory.</span></span>

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

<span data-ttu-id="1d3f5-234">**Azure Blob výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="1d3f5-234">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="1d3f5-235">Data jsou nový objekt blob zkopírovaný tooa každou hodinu s hello cesty pro objekt blob hello odrážející konkrétní datum a čas hello s rozlišením hodinu.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-235">Data is copied tooa new blob every hour with hello path for hello blob reflecting hello specific datetime with hour granularity.</span></span>

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
<span data-ttu-id="1d3f5-236">Ukázka dokumentu JSON v hello osoba kolekce v databázi Cosmos DB:</span><span class="sxs-lookup"><span data-stu-id="1d3f5-236">Sample JSON document in hello Person collection in a Cosmos DB database:</span></span>

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
<span data-ttu-id="1d3f5-237">Cosmos DB podporuje dotazování dokumentů pomocí SQL jako syntaxe přes hierarchické dokumentů JSON.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-237">Cosmos DB supports querying documents using a SQL like syntax over hierarchical JSON documents.</span></span>

<span data-ttu-id="1d3f5-238">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1d3f5-238">Example:</span></span> 

```sql
SELECT Person.PersonId, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person
```

<span data-ttu-id="1d3f5-239">Následující Hello kanál kopíruje data z hello osoba kolekce v hello tooan databáze Azure Cosmos DB objektů blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-239">hello following pipeline copies data from hello Person collection in hello Azure Cosmos DB database tooan Azure blob.</span></span> <span data-ttu-id="1d3f5-240">Jako součást hello kopie aktivity hello nebyly zadány vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-240">As part of hello copy activity hello input and output datasets have been specified.</span></span>  

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
## <a name="example-copy-data-from-azure-blob-tooazure-cosmos-db"></a><span data-ttu-id="1d3f5-241">Příklad: Kopírování dat z Azure Blob tooAzure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="1d3f5-241">Example: Copy data from Azure Blob tooAzure Cosmos DB</span></span> 
<span data-ttu-id="1d3f5-242">znázorňuje následující ukázka Hello:</span><span class="sxs-lookup"><span data-stu-id="1d3f5-242">hello sample below shows:</span></span>

1. <span data-ttu-id="1d3f5-243">Propojené služby typu [DocumentDb](#azure-documentdb-linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="1d3f5-243">A linked service of type [DocumentDb](#azure-documentdb-linked-service-properties).</span></span>
2. <span data-ttu-id="1d3f5-244">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="1d3f5-244">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="1d3f5-245">Vstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="1d3f5-245">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="1d3f5-246">Výstup [datovou sadu](data-factory-create-datasets.md) typu [DocumentDbCollection](#azure-documentdb-dataset-type-properties).</span><span class="sxs-lookup"><span data-stu-id="1d3f5-246">An output [dataset](data-factory-create-datasets.md) of type [DocumentDbCollection](#azure-documentdb-dataset-type-properties).</span></span>
5. <span data-ttu-id="1d3f5-247">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) a [DocumentDbCollectionSink](#azure-documentdb-copy-activity-type-properties).</span><span class="sxs-lookup"><span data-stu-id="1d3f5-247">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) and [DocumentDbCollectionSink](#azure-documentdb-copy-activity-type-properties).</span></span>

<span data-ttu-id="1d3f5-248">Ukázka Hello zkopíruje data z Azure blob tooAzure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-248">hello sample copies data from Azure blob tooAzure Cosmos DB.</span></span> <span data-ttu-id="1d3f5-249">Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-249">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="1d3f5-250">**Propojená služba Azure Blob storage:**</span><span class="sxs-lookup"><span data-stu-id="1d3f5-250">**Azure Blob storage linked service:**</span></span>

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
<span data-ttu-id="1d3f5-251">**Azure Cosmos DB propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="1d3f5-251">**Azure Cosmos DB linked service:**</span></span>

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
<span data-ttu-id="1d3f5-252">**Azure vstupní datovou sadu objektu Blob:**</span><span class="sxs-lookup"><span data-stu-id="1d3f5-252">**Azure Blob input dataset:**</span></span>

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
<span data-ttu-id="1d3f5-253">**Azure Cosmos DB výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="1d3f5-253">**Azure Cosmos DB output dataset:**</span></span>

<span data-ttu-id="1d3f5-254">Ukázka Hello zkopíruje data tooa kolekci s názvem "Osoba".</span><span class="sxs-lookup"><span data-stu-id="1d3f5-254">hello sample copies data tooa collection named “Person”.</span></span>

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
<span data-ttu-id="1d3f5-255">Následující Hello kanál kopíruje data z Azure Blob toohello osoba kolekce v hello Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-255">hello following pipeline copies data from Azure Blob toohello Person collection in hello Cosmos DB.</span></span> <span data-ttu-id="1d3f5-256">Jako součást hello kopie aktivity hello nebyly zadány vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-256">As part of hello copy activity hello input and output datasets have been specified.</span></span>

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
              "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, title: aaaTitle, Suffix: Suffix, EmailPromotion: EmailPromotion, rowguid: rowguid, ModifiedDate: ModifiedDate"
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
<span data-ttu-id="1d3f5-257">Pokud hello ukázkových objektů blob vstup je jako</span><span class="sxs-lookup"><span data-stu-id="1d3f5-257">If hello sample blob input is as</span></span>

```
1,John,,Doe
```
<span data-ttu-id="1d3f5-258">Výstup hello JSON v Cosmos DB pak bude jako:</span><span class="sxs-lookup"><span data-stu-id="1d3f5-258">Then hello output JSON in Cosmos DB will be as:</span></span>

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
<span data-ttu-id="1d3f5-259">Azure Cosmos DB je úložiště typu NoSQL pro dokumenty JSON, kde jsou povoleny vnořené struktury.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-259">Azure Cosmos DB is a NoSQL store for JSON documents, where nested structures are allowed.</span></span> <span data-ttu-id="1d3f5-260">Azure Data Factory umožňuje hierarchie toodenote uživatele prostřednictvím **nestingSeparator**, což je "."</span><span class="sxs-lookup"><span data-stu-id="1d3f5-260">Azure Data Factory enables user toodenote hierarchy via **nestingSeparator**, which is “.”</span></span> <span data-ttu-id="1d3f5-261">v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-261">in this example.</span></span> <span data-ttu-id="1d3f5-262">S oddělovačem hello aktivity kopírování hello vygeneruje hello "Name" objekt s tří podřízených elementů první, střední a poslední, podle too"Name.First", "Name.Middle" a "Name.Last" v hello Definice tabulky.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-262">With hello separator, hello copy activity will generate hello “Name” object with three children elements First, Middle and Last, according too“Name.First”, “Name.Middle” and “Name.Last” in hello table definition.</span></span>

## <a name="appendix"></a><span data-ttu-id="1d3f5-263">Příloha</span><span class="sxs-lookup"><span data-stu-id="1d3f5-263">Appendix</span></span>
1. <span data-ttu-id="1d3f5-264">**Otázka:** hello aktivity kopírování podporu aktualizace existující záznamy?</span><span class="sxs-lookup"><span data-stu-id="1d3f5-264">**Question:** Does hello Copy Activity support update of existing records?</span></span>

    <span data-ttu-id="1d3f5-265">**Odpověď:** ne.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-265">**Answer:** No.</span></span>
2. <span data-ttu-id="1d3f5-266">**Otázka:** jak již nemá opakování ke kopírování tooAzure Cosmos DB pozornosti s zkopírovat záznamy?</span><span class="sxs-lookup"><span data-stu-id="1d3f5-266">**Question:** How does a retry of a copy tooAzure Cosmos DB deal with already copied records?</span></span>

    <span data-ttu-id="1d3f5-267">**Odpověď:** Pokud záznamy na pole "ID" a operace kopírování hello pokusí tooinsert záznam s hello stejné ID operace kopírování hello vrátí chybu.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-267">**Answer:** If records have an "ID" field and hello copy operation tries tooinsert a record with hello same ID, hello copy operation throws an error.</span></span>  
3. <span data-ttu-id="1d3f5-268">**Otázka:** podporuje služby Data Factory [rozsah nebo dělení dat na základě hodnoty hash](../documentdb/documentdb-partition-data.md)?</span><span class="sxs-lookup"><span data-stu-id="1d3f5-268">**Question:** Does Data Factory support [range or hash-based data partitioning](../documentdb/documentdb-partition-data.md)?</span></span>

    <span data-ttu-id="1d3f5-269">**Odpověď:** ne.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-269">**Answer:** No.</span></span>
4. <span data-ttu-id="1d3f5-270">**Otázka:** můžete zadat více než jednu kolekci Azure Cosmos DB pro tabulku?</span><span class="sxs-lookup"><span data-stu-id="1d3f5-270">**Question:** Can I specify more than one Azure Cosmos DB collection for a table?</span></span>

    <span data-ttu-id="1d3f5-271">**Odpověď:** ne.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-271">**Answer:** No.</span></span> <span data-ttu-id="1d3f5-272">V tuto chvíli je možné zadat pouze jednu kolekci.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-272">Only one collection can be specified at this time.</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="1d3f5-273">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="1d3f5-273">Performance and Tuning</span></span>
<span data-ttu-id="1d3f5-274">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.</span><span class="sxs-lookup"><span data-stu-id="1d3f5-274">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
