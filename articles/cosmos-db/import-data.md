---
title: "Nástroj pro migraci databáze pro databázi Azure Cosmos | Microsoft Docs"
description: "Další informace o použití nástrojů pro migraci dat Azure Cosmos DB s otevřeným zdrojem pro import dat do Azure Cosmos databáze z různých zdrojů, včetně souborů MongoDB, systému SQL Server, úložiště Table, Amazon DynamoDB, CSV a JSON. CSV k převodu formátu JSON."
keywords: "CSV do formátu json, nástrojů pro migraci databáze, převést na json sdíleného svazku clusteru"
services: cosmos-db
author: andrewhoh
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: d173581d-782a-445c-98d9-5e3c49b00e25
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: anhoh
ms.custom: mvc
ms.openlocfilehash: 23a4a82dbdb611f4da90562af936fca28da9b24d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-import-data-into-azure-cosmos-db-for-the-documentdb-api"></a><span data-ttu-id="2fe1c-105">Jak importovat data do Azure Cosmos DB pro rozhraní API DocumentDB?</span><span class="sxs-lookup"><span data-stu-id="2fe1c-105">How to import data into Azure Cosmos DB for the DocumentDB API?</span></span>

<span data-ttu-id="2fe1c-106">V tomto kurzu poskytuje pokyny k používání Azure Cosmos DB: nástroj pro migraci dat rozhraní API DocumentDB, který můžete importovat data z různých zdrojů, včetně souborů JSON, CSV soubory, SQL, MongoDB, Azure Table storage, Amazon DynamoDB a rozhraní API služby Azure Cosmos databáze DocumentDB kolekce do kolekce pro použití s Azure Cosmos DB a rozhraní API DocumentDB.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-106">This tutorial provides instructions on using the Azure Cosmos DB: DocumentDB API Data Migration tool, which can import data from various sources, including JSON files, CSV files, SQL, MongoDB, Azure Table storage, Amazon DynamoDB and Azure Cosmos DB DocumentDB API collections into collections for use with Azure Cosmos DB and the DocumentDB API.</span></span> <span data-ttu-id="2fe1c-107">Nástroj pro migraci dat lze také při migraci z kolekce tvořené jedním oddílem kolekci více oddílů pro rozhraní API DocumentDB.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-107">The Data Migration tool can also be used when migrating from a single partition collection to a multi-partition collection for the DocumentDB API.</span></span>

<span data-ttu-id="2fe1c-108">Nástroj pro migraci dat funguje pouze v případě import dat do Azure DB Cosmos pro použití s rozhraním API pro DocumentDB.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-108">The Data Migration tool only works when importing data into Azure Cosmos DB for use with the DocumentDB API.</span></span> <span data-ttu-id="2fe1c-109">V tuto chvíli není podporován import dat pro použití se službou API tabulky nebo rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-109">Importing data for use with the Table API or Graph API is not supported at this time.</span></span> 

<span data-ttu-id="2fe1c-110">K importu dat pro použití s rozhraním API pro MongoDB, najdete v části [Cosmos databázi Azure: migrace dat pro rozhraní API MongoDB?](mongodb-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="2fe1c-110">To import data for use with the MongoDB API, see [Azure Cosmos DB: How to migrate data for the MongoDB API?](mongodb-migrate.md).</span></span>

<span data-ttu-id="2fe1c-111">Tento kurz obsahuje následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-111">This tutorial covers the following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2fe1c-112">Instalace nástroje pro migraci dat</span><span class="sxs-lookup"><span data-stu-id="2fe1c-112">Installing the Data Migration tool</span></span>
> * <span data-ttu-id="2fe1c-113">Import dat z různých zdrojů dat.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-113">Importing data from different data sources</span></span>
> * <span data-ttu-id="2fe1c-114">Export z Azure Cosmos DB do formátu JSON</span><span class="sxs-lookup"><span data-stu-id="2fe1c-114">Exporting from Azure Cosmos DB to JSON</span></span>

## <span data-ttu-id="2fe1c-115"><a id="Prerequisites"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="2fe1c-115"><a id="Prerequisites"></a>Prerequisites</span></span>
<span data-ttu-id="2fe1c-116">Než budete postupovat podle pokynů v tomto článku, ujistěte se, že máte nainstalované tyto položky:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-116">Before following the instructions in this article, ensure that you have the following installed:</span></span>

* <span data-ttu-id="2fe1c-117">[Rozhraní Microsoft .NET Framework 4.51](https://www.microsoft.com/download/developer-tools.aspx) nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-117">[Microsoft .NET Framework 4.51](https://www.microsoft.com/download/developer-tools.aspx) or higher.</span></span>

## <span data-ttu-id="2fe1c-118"><a id="Overviewl"></a>Přehled nástroje pro migraci dat</span><span class="sxs-lookup"><span data-stu-id="2fe1c-118"><a id="Overviewl"></a>Overview of the Data Migration tool</span></span>
<span data-ttu-id="2fe1c-119">Nástroj pro migraci dat je řešení open source, který importuje data do Azure Cosmos databáze z různých zdrojů, včetně:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-119">The Data Migration tool is an open source solution that imports data to Azure Cosmos DB from a variety of sources, including:</span></span>

* <span data-ttu-id="2fe1c-120">Soubory JSON</span><span class="sxs-lookup"><span data-stu-id="2fe1c-120">JSON files</span></span>
* <span data-ttu-id="2fe1c-121">MongoDB</span><span class="sxs-lookup"><span data-stu-id="2fe1c-121">MongoDB</span></span>
* <span data-ttu-id="2fe1c-122">SQL Server</span><span class="sxs-lookup"><span data-stu-id="2fe1c-122">SQL Server</span></span>
* <span data-ttu-id="2fe1c-123">CSV soubory</span><span class="sxs-lookup"><span data-stu-id="2fe1c-123">CSV files</span></span>
* <span data-ttu-id="2fe1c-124">Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="2fe1c-124">Azure Table storage</span></span>
* <span data-ttu-id="2fe1c-125">Amazon DynamoDB</span><span class="sxs-lookup"><span data-stu-id="2fe1c-125">Amazon DynamoDB</span></span>
* <span data-ttu-id="2fe1c-126">HBase</span><span class="sxs-lookup"><span data-stu-id="2fe1c-126">HBase</span></span>
* <span data-ttu-id="2fe1c-127">Azure Cosmos DB kolekce</span><span class="sxs-lookup"><span data-stu-id="2fe1c-127">Azure Cosmos DB collections</span></span>

<span data-ttu-id="2fe1c-128">Při importu nástroj obsahuje grafické uživatelské rozhraní (dtui.exe), můžete také vycházejí z příkazového řádku (dt.exe).</span><span class="sxs-lookup"><span data-stu-id="2fe1c-128">While the import tool includes a graphical user interface (dtui.exe), it can also be driven from the command line (dt.exe).</span></span> <span data-ttu-id="2fe1c-129">Ve skutečnosti je možnost výstup přidružený příkaz po nastavení importu v uživatelském rozhraní.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-129">In fact, there is an option to output the associated command after setting up an import through the UI.</span></span> <span data-ttu-id="2fe1c-130">Tabulkové zdroje dat (např. SQL Server nebo CSV soubory) lze je transformovat tak, aby hierarchické vztahy (vnořené dokumenty) lze vytvořit při importu.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-130">Tabular source data (e.g. SQL Server or CSV files) can be transformed such that hierarchical relationships (subdocuments) can be created during import.</span></span> <span data-ttu-id="2fe1c-131">Zachovat čtení Další informace o možnosti nastavení zdroje, ukázkové příkazové řádky pro import z každého zdroje, target – možnosti a import zobrazení výsledků.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-131">Keep reading to learn more about source options, sample command lines to import from each source, target options, and viewing import results.</span></span>

## <span data-ttu-id="2fe1c-132"><a id="Install"></a>Nainstalujte nástroj pro migraci dat</span><span class="sxs-lookup"><span data-stu-id="2fe1c-132"><a id="Install"></a>Install the Data Migration tool</span></span>
<span data-ttu-id="2fe1c-133">Migrace nástroj zdrojový kód je k dispozici na Githubu v [toto úložiště](https://github.com/azure/azure-documentdb-datamigrationtool) a je k dispozici z kompilované verze [Microsoft Download Center](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d).</span><span class="sxs-lookup"><span data-stu-id="2fe1c-133">The migration tool source code is available on GitHub in [this repository](https://github.com/azure/azure-documentdb-datamigrationtool) and a compiled version is available from [Microsoft Download Center](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d).</span></span> <span data-ttu-id="2fe1c-134">Může buď zkompilovat řešení nebo jednoduše stažení a extrakci kompilované verze k adresáři podle vašeho výběru.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-134">You may either compile the solution or simply download and extract the compiled version to a directory of your choice.</span></span> <span data-ttu-id="2fe1c-135">Spusťte buď:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-135">Then run either:</span></span>

* <span data-ttu-id="2fe1c-136">**Dtui.exe**: grafické rozhraní verze nástroje</span><span class="sxs-lookup"><span data-stu-id="2fe1c-136">**Dtui.exe**: Graphical interface version of the tool</span></span>
* <span data-ttu-id="2fe1c-137">**DT.exe**: příkazového řádku verze nástroje</span><span class="sxs-lookup"><span data-stu-id="2fe1c-137">**Dt.exe**: Command-line version of the tool</span></span>

## <a name="import-data"></a><span data-ttu-id="2fe1c-138">Import dat</span><span class="sxs-lookup"><span data-stu-id="2fe1c-138">Import data</span></span>

<span data-ttu-id="2fe1c-139">Po instalaci tohoto nástroje je čas lze importovat data.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-139">Once you've installed the tool, it's time to import your data.</span></span> <span data-ttu-id="2fe1c-140">Jaký typ dat chcete importovat?</span><span class="sxs-lookup"><span data-stu-id="2fe1c-140">What kind of data do you want to import?</span></span>

* [<span data-ttu-id="2fe1c-141">Soubory JSON</span><span class="sxs-lookup"><span data-stu-id="2fe1c-141">JSON files</span></span>](#JSON)
* [<span data-ttu-id="2fe1c-142">MongoDB</span><span class="sxs-lookup"><span data-stu-id="2fe1c-142">MongoDB</span></span>](#MongoDB)
* [<span data-ttu-id="2fe1c-143">MongoDB exportní soubory</span><span class="sxs-lookup"><span data-stu-id="2fe1c-143">MongoDB Export files</span></span>](#MongoDBExport)
* [<span data-ttu-id="2fe1c-144">SQL Server</span><span class="sxs-lookup"><span data-stu-id="2fe1c-144">SQL Server</span></span>](#SQL)
* [<span data-ttu-id="2fe1c-145">CSV soubory</span><span class="sxs-lookup"><span data-stu-id="2fe1c-145">CSV files</span></span>](#CSV)
* [<span data-ttu-id="2fe1c-146">Azure Table storage</span><span class="sxs-lookup"><span data-stu-id="2fe1c-146">Azure Table storage</span></span>](#AzureTableSource)
* [<span data-ttu-id="2fe1c-147">Amazon DynamoDB</span><span class="sxs-lookup"><span data-stu-id="2fe1c-147">Amazon DynamoDB</span></span>](#DynamoDBSource)
* [<span data-ttu-id="2fe1c-148">Objekt BLOB</span><span class="sxs-lookup"><span data-stu-id="2fe1c-148">Blob</span></span>](#BlobImport)
* [<span data-ttu-id="2fe1c-149">Azure Cosmos DB kolekce</span><span class="sxs-lookup"><span data-stu-id="2fe1c-149">Azure Cosmos DB collections</span></span>](#DocumentDBSource)
* [<span data-ttu-id="2fe1c-150">HBase</span><span class="sxs-lookup"><span data-stu-id="2fe1c-150">HBase</span></span>](#HBaseSource)
* [<span data-ttu-id="2fe1c-151">Azure Cosmos DB hromadného importu</span><span class="sxs-lookup"><span data-stu-id="2fe1c-151">Azure Cosmos DB bulk import</span></span>](#DocumentDBBulkImport)
* [<span data-ttu-id="2fe1c-152">Sekvenční záznam importu do Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="2fe1c-152">Azure Cosmos DB sequential record import</span></span>](#DocumentDSeqTarget)


## <span data-ttu-id="2fe1c-153"><a id="JSON"></a>Chcete-li importovat soubory JSON</span><span class="sxs-lookup"><span data-stu-id="2fe1c-153"><a id="JSON"></a>To import JSON files</span></span>
<span data-ttu-id="2fe1c-154">Možnost program pro import zdrojového souboru JSON umožňuje importovat jeden nebo více jednotlivý dokument JSON soubory nebo soubory JSON, že každý obsahovat pole dokumentů JSON.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-154">The JSON file source importer option allows you to import one or more single document JSON files or JSON files that each contain an array of JSON documents.</span></span> <span data-ttu-id="2fe1c-155">Při přidávání složek, které obsahují soubory JSON pro import, máte možnost rekurzivní hledání souborů v podsložkách.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-155">When adding folders that contain JSON files to import, you have the option of recursively searching for files in subfolders.</span></span>

![Možnosti nastavení zdroje souboru snímek JSON - nástrojů pro migraci databáze](./media/import-data/jsonsource.png)

<span data-ttu-id="2fe1c-157">Zde jsou některé ukázky příkazového řádku k importu souborů JSON:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-157">Here are some command line samples to import JSON files:</span></span>

    #Import a single JSON file
    dt.exe /s:JsonFile /s.Files:.\Sessions.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Sessions /t.CollectionThroughput:2500

    #Import a directory of JSON files
    dt.exe /s:JsonFile /s.Files:C:\TESessions\*.json /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Sessions /t.CollectionThroughput:2500

    #Import a directory (including sub-directories) of JSON files
    dt.exe /s:JsonFile /s.Files:C:\LastFMMusic\**\*.json /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Music /t.CollectionThroughput:2500

    #Import a directory (single), directory (recursive), and individual JSON files
    dt.exe /s:JsonFile /s.Files:C:\Tweets\*.*;C:\LargeDocs\**\*.*;C:\TESessions\Session48172.json;C:\TESessions\Session48173.json;C:\TESessions\Session48174.json;C:\TESessions\Session48175.json;C:\TESessions\Session48177.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:subs /t.CollectionThroughput:2500

    #Import a single JSON file and partition the data across 4 collections
    dt.exe /s:JsonFile /s.Files:D:\\CompanyData\\Companies.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:comp[1-4] /t.PartitionKey:name /t.CollectionThroughput:2500

## <span data-ttu-id="2fe1c-158"><a id="MongoDB"></a>Chcete-li importovat z MongoDB</span><span class="sxs-lookup"><span data-stu-id="2fe1c-158"><a id="MongoDB"></a>To import from MongoDB</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2fe1c-159">Pokud importujete k účtu Azure Cosmos DB s podporou pro MongoDB, postupujte podle těchto [pokyny](mongodb-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="2fe1c-159">If you are importing to an Azure Cosmos DB account with Support for MongoDB, follow these [instructions](mongodb-migrate.md).</span></span>
> 
> 

<span data-ttu-id="2fe1c-160">Možnost MongoDB zdrojový program pro import můžete importovat z kolekci jednotlivých MongoDB a volitelně filtrovat dokumentů pomocí dotazu nebo změna struktury dokumentu pomocí projekce.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-160">The MongoDB source importer option allows you to import from an individual MongoDB collection and optionally filter documents using a query and/or modify the document structure by using a projection.</span></span>  

![Možnosti nastavení zdroje snímek MongoDB](./media/import-data/mongodbsource.png)

<span data-ttu-id="2fe1c-162">Připojovací řetězec je ve formátu standard MongoDB:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-162">The connection string is in the standard MongoDB format:</span></span>

    mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database>

> [!NOTE]
> <span data-ttu-id="2fe1c-163">Pomocí příkazu ověřte, zda je přístupná instance MongoDB, zadaný v poli.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-163">Use the Verify command to ensure that the MongoDB instance specified in the connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="2fe1c-164">Zadejte název kolekce, ze kterého budou importována data.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-164">Enter the name of the collection from which data will be imported.</span></span> <span data-ttu-id="2fe1c-165">Může volitelně zadejte nebo zadejte soubor pro dotaz (například {pop: {$gt: 5000}}) nebo projekce (například {loc:0}) pro filtrování i data, která bude importována obrazce.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-165">You may optionally specify or provide a file for a query (e.g. {pop: {$gt:5000}} ) and/or projection (e.g. {loc:0} ) to both filter and shape the data to be imported.</span></span>

<span data-ttu-id="2fe1c-166">Zde jsou některé ukázky příkazového řádku k importu z MongoDB:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-166">Here are some command line samples to import from MongoDB:</span></span>

    #Import all documents from a MongoDB collection
    dt.exe /s:MongoDB /s.ConnectionString:mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database> /s.Collection:zips /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:BulkZips /t.IdField:_id /t.CollectionThroughput:2500

    #Import documents from a MongoDB collection which match the query and exclude the loc field
    dt.exe /s:MongoDB /s.ConnectionString:mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database> /s.Collection:zips /s.Query:{pop:{$gt:50000}} /s.Projection:{loc:0} /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:BulkZipsTransform /t.IdField:_id/t.CollectionThroughput:2500

## <span data-ttu-id="2fe1c-167"><a id="MongoDBExport"></a>Chcete-li importovat MongoDB exportní soubory</span><span class="sxs-lookup"><span data-stu-id="2fe1c-167"><a id="MongoDBExport"></a>To import MongoDB export files</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2fe1c-168">Pokud importujete k účtu Azure Cosmos DB s podporou pro MongoDB, postupujte podle těchto [pokyny](mongodb-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="2fe1c-168">If you are importing to an Azure Cosmos DB account with support for MongoDB, follow these [instructions](mongodb-migrate.md).</span></span>
> 
> 

<span data-ttu-id="2fe1c-169">Program pro import možnost MongoDB export souboru JSON zdroj umožňuje importovat jeden nebo více souborů JSON vytvořeného z nástroj mongoexport.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-169">The MongoDB export JSON file source importer option allows you to import one or more JSON files produced from the mongoexport utility.</span></span>  

![Možnosti nastavení zdroje export snímek MongoDB](./media/import-data/mongodbexportsource.png)

<span data-ttu-id="2fe1c-171">Při přidávání složek, které obsahují soubory JSON export MongoDB pro import, máte možnost rekurzivní hledání souborů v podsložkách.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-171">When adding folders that contain MongoDB export JSON files for import, you have the option of recursively searching for files in subfolders.</span></span>

<span data-ttu-id="2fe1c-172">Zde je ukázka příkazového řádku k importu z soubory JSON export MongoDB:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-172">Here is a command line sample to import from MongoDB export JSON files:</span></span>

    dt.exe /s:MongoDBExport /s.Files:D:\mongoemployees.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:employees /t.IdField:_id /t.Dates:Epoch /t.CollectionThroughput:2500

## <span data-ttu-id="2fe1c-173"><a id="SQL"></a>Import ze systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="2fe1c-173"><a id="SQL"></a>To import from SQL Server</span></span>
<span data-ttu-id="2fe1c-174">Možnost program pro import zdroje SQL umožňuje importovat z jednotlivých databáze systému SQL Server a volitelně filtrování záznamů k importu pomocí dotazu.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-174">The SQL source importer option allows you to import from an individual SQL Server database and optionally filter the records to be imported using a query.</span></span> <span data-ttu-id="2fe1c-175">Kromě toho můžete upravit strukturu dokumentu zadáním vnoření oddělovače (podrobnosti o této za chvíli).</span><span class="sxs-lookup"><span data-stu-id="2fe1c-175">In addition, you can modify the document structure by specifying a nesting separator (more on that in a moment).</span></span>  

![Možnosti nastavení zdroje snímek SQL – nástroje pro migraci databáze](./media/import-data/sqlexportsource.png)

<span data-ttu-id="2fe1c-177">Formát připojovacího řetězce je standardní formátu řetězce připojení SQL.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-177">The format of the connection string is the standard SQL connection string format.</span></span>

> [!NOTE]
> <span data-ttu-id="2fe1c-178">Pomocí příkazu ověřte Ujistěte se, že instance systému SQL Server zadané v poli je přístupná.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-178">Use the Verify command to ensure that the SQL Server instance specified in the connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="2fe1c-179">Vnoření oddělovače vlastnost se používá k vytvoření hierarchické relací (dílčí dokumenty) během importu.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-179">The nesting separator property is used to create hierarchical relationships (sub-documents) during import.</span></span> <span data-ttu-id="2fe1c-180">Vezměte v úvahu následujícího dotazu SQL:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-180">Consider the following SQL query:</span></span>

<span data-ttu-id="2fe1c-181">*jako Id, názvu, AddressType jako [Address.AddressType], AddressLine1 jako [Address.AddressLine1], města jako [Address.Location.City], StateProvinceName jako [Address.Location.StateProvinceName], PostalCode jako [[[vyberte PŘETYPOVÁNÍ (BusinessEntityID AS varchar) Address.PostalCode] CountryRegionName jako [Address.CountryRegionName] z Sales.vStoreWithAddresses kde AddressType = "hlavní kanceláře.*</span><span class="sxs-lookup"><span data-stu-id="2fe1c-181">*select CAST(BusinessEntityID AS varchar) as Id, Name, AddressType as [Address.AddressType], AddressLine1 as [Address.AddressLine1], City as [Address.Location.City], StateProvinceName as [Address.Location.StateProvinceName], PostalCode as [Address.PostalCode], CountryRegionName as [Address.CountryRegionName] from Sales.vStoreWithAddresses WHERE AddressType='Main Office'*</span></span>

<span data-ttu-id="2fe1c-182">Která vrací následující výsledky (částečné):</span><span class="sxs-lookup"><span data-stu-id="2fe1c-182">Which returns the following (partial) results:</span></span>

![Snímek obrazovky SQL výsledky dotazu](./media/import-data/sqlqueryresults.png)

<span data-ttu-id="2fe1c-184">Poznámka: aliasy například Address.AddressType a Address.Location.StateProvinceName.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-184">Note the aliases such as Address.AddressType and Address.Location.StateProvinceName.</span></span> <span data-ttu-id="2fe1c-185">Zadáním oddělovač vnoření z '.', nástroj pro import během importu vytvoří adresu a Address.Location vnořených dokumentů.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-185">By specifying a nesting separator of ‘.’, the import tool creates Address and Address.Location subdocuments during the import.</span></span> <span data-ttu-id="2fe1c-186">Tady je příklad výsledné dokumentu v Azure Cosmos DB:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-186">Here is an example of a resulting document in Azure Cosmos DB:</span></span>

<span data-ttu-id="2fe1c-187">*{"id": "956", "název": "Jemnějšího prodeje a služba", "Adres": {"AddressType": "Hlavní Office", "AddressLine1": "#500 75 O'Connor ulici", "Umístění": {"City": "Ottawa", "StateProvinceName": "Ontario"}, "PostalCode": "K4B 1S2", "CountryRegionName": " Kanada"}}*</span><span class="sxs-lookup"><span data-stu-id="2fe1c-187">*{ "id": "956", "Name": "Finer Sales and Service", "Address": { "AddressType": "Main Office", "AddressLine1": "#500-75 O'Connor Street", "Location": { "City": "Ottawa", "StateProvinceName": "Ontario" }, "PostalCode": "K4B 1S2", "CountryRegionName": "Canada" } }*</span></span>

<span data-ttu-id="2fe1c-188">Zde jsou některé ukázky příkazového řádku k importu ze serveru SQL Server:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-188">Here are some command line samples to import from SQL Server:</span></span>

    #Import records from SQL which match a query
    dt.exe /s:SQL /s.ConnectionString:"Data Source=<server>;Initial Catalog=AdventureWorks;User Id=advworks;Password=<password>;" /s.Query:"select CAST(BusinessEntityID AS varchar) as Id, * from Sales.vStoreWithAddresses WHERE AddressType='Main Office'" /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Stores /t.IdField:Id /t.CollectionThroughput:2500

    #Import records from sql which match a query and create hierarchical relationships
    dt.exe /s:SQL /s.ConnectionString:"Data Source=<server>;Initial Catalog=AdventureWorks;User Id=advworks;Password=<password>;" /s.Query:"select CAST(BusinessEntityID AS varchar) as Id, Name, AddressType as [Address.AddressType], AddressLine1 as [Address.AddressLine1], City as [Address.Location.City], StateProvinceName as [Address.Location.StateProvinceName], PostalCode as [Address.PostalCode], CountryRegionName as [Address.CountryRegionName] from Sales.vStoreWithAddresses WHERE AddressType='Main Office'" /s.NestingSeparator:. /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:StoresSub /t.IdField:Id /t.CollectionThroughput:2500

## <span data-ttu-id="2fe1c-189"><a id="CSV"></a>K importu souborů sdíleného svazku clusteru a CSV převést na JSON</span><span class="sxs-lookup"><span data-stu-id="2fe1c-189"><a id="CSV"></a>To import CSV files and convert CSV to JSON</span></span>
<span data-ttu-id="2fe1c-190">Možnost – Importér zdrojového souboru CSV umožňuje importovat jeden nebo více souborů CSV.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-190">The CSV file source importer option enables you to import one or more CSV files.</span></span> <span data-ttu-id="2fe1c-191">Při přidávání složek, které obsahují soubory sdíleného svazku clusteru pro import, máte možnost rekurzivní hledání souborů v podsložkách.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-191">When adding folders that contain CSV files for import, you have the option of recursively searching for files in subfolders.</span></span>

![Možnosti nastavení zdroje snímek CSV - CSV do formátu JSON](media/import-data/csvsource.png)

<span data-ttu-id="2fe1c-193">Podobně jako u zdroje SQL, vlastnost vnoření oddělovače lze vytvořit hierarchické relace (dílčí dokumenty) během importu.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-193">Similar to the SQL source, the nesting separator property may be used to create hierarchical relationships (sub-documents) during import.</span></span> <span data-ttu-id="2fe1c-194">Zvažte následující CSV záhlaví řádků a data řádky:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-194">Consider the following CSV header row and data rows:</span></span>

![Snímek obrazovky CSV ukázkové záznamy - CSV do formátu JSON](./media/import-data/csvsample.png)

<span data-ttu-id="2fe1c-196">Poznámka: aliasy například DomainInfo.Domain_Name a RedirectInfo.Redirecting.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-196">Note the aliases such as DomainInfo.Domain_Name and RedirectInfo.Redirecting.</span></span> <span data-ttu-id="2fe1c-197">Zadáním oddělovač vnoření z '.', nástroj pro import vytvoří během importu DomainInfo a RedirectInfo vnořených dokumentů.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-197">By specifying a nesting separator of ‘.’, the import tool will create DomainInfo and RedirectInfo subdocuments during the import.</span></span> <span data-ttu-id="2fe1c-198">Tady je příklad výsledné dokumentu v Azure Cosmos DB:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-198">Here is an example of a resulting document in Azure Cosmos DB:</span></span>

<span data-ttu-id="2fe1c-199">*{"DomainInfo": {"Domain_Name": "ACUS.GOV", "Domain_Name_Address": "http://www.ACUS.GOV"}, "Federal agenturou": "správu konferenční z USA", "RedirectInfo": {"Přesměrování": "0", "Redirect_Destination": ""}, "id": "9cc565c5-ebcd-1c03-ebd3-cc3e2ecd814d"}*</span><span class="sxs-lookup"><span data-stu-id="2fe1c-199">*{ "DomainInfo": { "Domain_Name": "ACUS.GOV", "Domain_Name_Address": "http://www.ACUS.GOV" }, "Federal Agency": "Administrative Conference of the United States", "RedirectInfo": { "Redirecting": "0", "Redirect_Destination": "" }, "id": "9cc565c5-ebcd-1c03-ebd3-cc3e2ecd814d" }*</span></span>

<span data-ttu-id="2fe1c-200">Nástroj pro import se pokusí o odvození informací o typu nekotovaných hodnoty v souborů CSV (hodnoty v uvozovkách se vždy pracuje jako řetězce).</span><span class="sxs-lookup"><span data-stu-id="2fe1c-200">The import tool will attempt to infer type information for unquoted values in CSV files (quoted values are always treated as strings).</span></span>  <span data-ttu-id="2fe1c-201">Typy jsou označeny v následujícím pořadí: číslo, datum a čas, logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-201">Types are identified in the following order: number, datetime, boolean.</span></span>  

<span data-ttu-id="2fe1c-202">Existují dvě věci, které je třeba o importu souboru CSV:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-202">There are two other things to note about CSV import:</span></span>

1. <span data-ttu-id="2fe1c-203">Ve výchozím nastavení, jsou nekotovaných hodnoty vždy oříznut pro karty a prostory, zatímco se zachovají hodnoty v uvozovkách jako-je.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-203">By default, unquoted values are always trimmed for tabs and spaces, while quoted values are preserved as-is.</span></span> <span data-ttu-id="2fe1c-204">Toto chování lze přepsat pomocí zaškrtávacího políčka Trim hodnoty v uvozovkách nebo /s.TrimQuoted možnost příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-204">This behavior can be overridden with the Trim quoted values checkbox or the /s.TrimQuoted command line option.</span></span>
2. <span data-ttu-id="2fe1c-205">Ve výchozím nastavení nekotovaných null považován za hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-205">By default, an unquoted null is treated as a null value.</span></span> <span data-ttu-id="2fe1c-206">Toto chování můžete přepsat (tj. považovat za nekotovaných null řetězec "null") se zpracovávat nekotované jako řetězec zaškrtávací políčko nebo možnost příkazového řádku /s.NoUnquotedNulls hodnotu NULL.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-206">This behavior can be overridden (i.e. treat an unquoted null as a “null” string) with the Treat unquoted NULL as string checkbox or the /s.NoUnquotedNulls command line option.</span></span>

<span data-ttu-id="2fe1c-207">Zde je ukázka příkazového řádku pro importu souboru CSV:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-207">Here is a command line sample for CSV import:</span></span>

    dt.exe /s:CsvFile /s.Files:.\Employees.csv /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Employees /t.IdField:EntityID /t.CollectionThroughput:2500

## <span data-ttu-id="2fe1c-208"><a id="AzureTableSource"></a>Chcete-li importovat z úložiště tabulek Azure</span><span class="sxs-lookup"><span data-stu-id="2fe1c-208"><a id="AzureTableSource"></a>To import from Azure Table storage</span></span>
<span data-ttu-id="2fe1c-209">Možnost zdroje program pro import úložiště Azure Table umožňuje importovat z jednotlivé tabulky Azure Table storage a volitelně filtrování entity tabulky určených k importu.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-209">The Azure Table storage source importer option allows you to import from an individual Azure Table storage table and optionally filter the table entities to be imported.</span></span> <span data-ttu-id="2fe1c-210">Všimněte si, že nástroj pro migraci dat nelze použít k importu dat Azure Table storage do Azure Cosmos DB pro použití s rozhraním API pro tabulku.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-210">Note that you cannot use the Data Migration tool to import Azure Table storage data into Azure Cosmos DB for use with the Table API.</span></span> <span data-ttu-id="2fe1c-211">V tuto chvíli je podporována pouze pro použití s rozhraním API pro DocumentDB importu do Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-211">Only importing to Azure Cosmos DB for use with the DocumentDB API is supported at this time.</span></span>

![Možnosti nastavení zdroje snímek Azure Table storage](./media/import-data/azuretablesource.png)

<span data-ttu-id="2fe1c-213">Formát připojovacího řetězce Azure Table storage je:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-213">The format of the Azure Table storage connection string is:</span></span>

    DefaultEndpointsProtocol=<protocol>;AccountName=<Account Name>;AccountKey=<Account Key>;

> [!NOTE]
> <span data-ttu-id="2fe1c-214">Pomocí příkazu ověřte, zda je přístupná zadané v poli instanci úložiště Azure Table.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-214">Use the Verify command to ensure that the Azure Table storage instance specified in the connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="2fe1c-215">Zadejte název tabulky Azure, ze kterého budou importována data.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-215">Enter the name of the Azure table from which data will be imported.</span></span> <span data-ttu-id="2fe1c-216">Volitelně můžete určit [filtru](https://msdn.microsoft.com/library/azure/ff683669.aspx).</span><span class="sxs-lookup"><span data-stu-id="2fe1c-216">You may optionally specify a [filter](https://msdn.microsoft.com/library/azure/ff683669.aspx).</span></span>

<span data-ttu-id="2fe1c-217">Možnost zdroje program pro import úložiště Azure Table obsahuje následující další možnosti:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-217">The Azure Table storage source importer option has the following additional options:</span></span>

1. <span data-ttu-id="2fe1c-218">Zahrnutí interní polí</span><span class="sxs-lookup"><span data-stu-id="2fe1c-218">Include Internal Fields</span></span>
   1. <span data-ttu-id="2fe1c-219">Všechny - obsahovat všechny interní pole (klíč oddílu, RowKey a časové razítko)</span><span class="sxs-lookup"><span data-stu-id="2fe1c-219">All - Include all internal fields (PartitionKey, RowKey, and Timestamp)</span></span>
   2. <span data-ttu-id="2fe1c-220">Žádný - vyloučit všechny interní pole</span><span class="sxs-lookup"><span data-stu-id="2fe1c-220">None - Exclude all internal fields</span></span>
   3. <span data-ttu-id="2fe1c-221">RowKey – obsahovat pouze pole RowKey</span><span class="sxs-lookup"><span data-stu-id="2fe1c-221">RowKey - Only include the RowKey field</span></span>
2. <span data-ttu-id="2fe1c-222">Vyberte sloupce</span><span class="sxs-lookup"><span data-stu-id="2fe1c-222">Select Columns</span></span>
   1. <span data-ttu-id="2fe1c-223">Azure Table storage filtry nepodporují projekce.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-223">Azure Table storage filters do not support projections.</span></span> <span data-ttu-id="2fe1c-224">Pokud chcete importovat pouze vlastnosti entity specifické Azure Table, přidejte je do seznamu vyberte sloupce.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-224">If you want to only import specific Azure Table entity properties, add them to the Select Columns list.</span></span> <span data-ttu-id="2fe1c-225">Všechny ostatní vlastnosti entity, bude ignorována.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-225">All other entity properties will be ignored.</span></span>

<span data-ttu-id="2fe1c-226">Zde je ukázka příkazového řádku k importu z úložiště tabulek Azure:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-226">Here is a command line sample to import from Azure Table storage:</span></span>

    dt.exe /s:AzureTable /s.ConnectionString:"DefaultEndpointsProtocol=https;AccountName=<Account Name>;AccountKey=<Account Key>" /s.Table:metrics /s.InternalFields:All /s.Filter:"PartitionKey eq 'Partition1' and RowKey gt '00001'" /s.Projection:ObjectCount;ObjectSize  /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:metrics /t.CollectionThroughput:2500

## <span data-ttu-id="2fe1c-227"><a id="DynamoDBSource"></a>Chcete-li importovat z Amazon DynamoDB</span><span class="sxs-lookup"><span data-stu-id="2fe1c-227"><a id="DynamoDBSource"></a>To import from Amazon DynamoDB</span></span>
<span data-ttu-id="2fe1c-228">Možnost Amazon DynamoDB zdrojový program pro import můžete importovat z jednotlivé tabulky Amazon DynamoDB a volitelně filtrování entity určených k importu.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-228">The Amazon DynamoDB source importer option allows you to import from an individual Amazon DynamoDB table and optionally filter the entities to be imported.</span></span> <span data-ttu-id="2fe1c-229">Tak, aby nastavení importu se co největší jsou k dispozici několik šablon.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-229">Several templates are provided so that setting up an import is as easy as possible.</span></span>

![Možnosti nastavení zdroje DynamoDB Amazon – snímek obrazovky - nástrojů pro migraci databáze](./media/import-data/dynamodbsource1.png)

![Možnosti nastavení zdroje DynamoDB Amazon – snímek obrazovky - nástrojů pro migraci databáze](./media/import-data/dynamodbsource2.png)

<span data-ttu-id="2fe1c-232">Formát Amazon DynamoDB připojovacího řetězce je:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-232">The format of the Amazon DynamoDB connection string is:</span></span>

    ServiceURL=<Service Address>;AccessKey=<Access Key>;SecretKey=<Secret Key>;

> [!NOTE]
> <span data-ttu-id="2fe1c-233">Pomocí příkazu ověřte, zda je přístupná zadané v poli instanci Amazon DynamoDB.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-233">Use the Verify command to ensure that the Amazon DynamoDB instance specified in the connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="2fe1c-234">Zde je ukázka příkazového řádku k importu z Amazon DynamoDB:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-234">Here is a command line sample to import from Amazon DynamoDB:</span></span>

    dt.exe /s:DynamoDB /s.ConnectionString:ServiceURL=https://dynamodb.us-east-1.amazonaws.com;AccessKey=<accessKey>;SecretKey=<secretKey> /s.Request:"{   """TableName""": """ProductCatalog""" }" /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<Azure Cosmos DB Endpoint>;AccountKey=<Azure Cosmos DB Key>;Database=<Azure Cosmos DB Database>;" /t.Collection:catalogCollection /t.CollectionThroughput:2500

## <span data-ttu-id="2fe1c-235"><a id="BlobImport"></a>K importu souborů z úložiště objektů Blob v Azure</span><span class="sxs-lookup"><span data-stu-id="2fe1c-235"><a id="BlobImport"></a>To import files from Azure Blob storage</span></span>
<span data-ttu-id="2fe1c-236">Soubor JSON, MongoDB export souboru a možnosti program pro import zdrojového souboru CSV umožňují importovat jeden nebo více souborů z úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-236">The JSON file, MongoDB export file, and CSV file source importer options allow you to import one or more files from Azure Blob storage.</span></span> <span data-ttu-id="2fe1c-237">Po zadání adresy URL kontejneru objektu Blob a klíč účtu, stačí zadejte regulární výraz k vyberte soubory, které chcete importovat.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-237">After specifying a Blob container URL and Account Key, simply provide a regular expression to select the file(s) to import.</span></span>

![Možnosti nastavení zdroje snímek objektu Blob souboru](./media/import-data/blobsource.png)

<span data-ttu-id="2fe1c-239">Zde je ukázka příkazového řádku k importu souborů JSON z Azure Blob storage:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-239">Here is command line sample to import JSON files from Azure Blob storage:</span></span>

    dt.exe /s:JsonFile /s.Files:"blobs://<account key>@account.blob.core.windows.net:443/importcontainer/.*" /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:doctest

## <span data-ttu-id="2fe1c-240"><a id="DocumentDBSource"></a>Chcete-li importovat z kolekce rozhraní API služby Azure Cosmos databáze DocumentDB</span><span class="sxs-lookup"><span data-stu-id="2fe1c-240"><a id="DocumentDBSource"></a>To import from an Azure Cosmos DB DocumentDB API collection</span></span>
<span data-ttu-id="2fe1c-241">Možnost Azure Cosmos DB zdrojový program pro import umožňuje importovat data z jedné nebo více kolekcí Azure Cosmos DB a volitelně filtrování dokumentů pomocí dotazu.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-241">The Azure Cosmos DB source importer option allows you to import data from one or more Azure Cosmos DB collections and optionally filter documents using a query.</span></span>  

![Možnosti nastavení zdroje snímek databázi Cosmos Azure](./media/import-data/documentdbsource.png)

<span data-ttu-id="2fe1c-243">Formát Azure Cosmos DB připojovacího řetězce je:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-243">The format of the Azure Cosmos DB connection string is:</span></span>

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

<span data-ttu-id="2fe1c-244">Připojovací řetězce k účtu Azure Cosmos DB lze načíst z okna klíče na portálu Azure, jak je popsáno v [Správa účtu Azure Cosmos DB](manage-account.md), ale název databáze musí být připojenou k připojení řetězec v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-244">The Azure Cosmos DB account connection string can be retrieved from the Keys blade of the Azure portal, as described in [How to manage an Azure Cosmos DB account](manage-account.md), however the name of the database needs to be appended to the connection string in the following format:</span></span>

    Database=<CosmosDB Database>;

> [!NOTE]
> <span data-ttu-id="2fe1c-245">Pomocí příkazu ověřte, zda je přístupná zadané v poli instanci Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-245">Use the Verify command to ensure that the Azure Cosmos DB instance specified in the connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="2fe1c-246">Pokud chcete importovat z jedné kolekce Azure Cosmos DB, zadejte název kolekce, ze kterého budou importována data.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-246">To import from a single Azure Cosmos DB collection, enter the name of the collection from which data will be imported.</span></span> <span data-ttu-id="2fe1c-247">Chcete-li importovat z více kolekcí Azure Cosmos DB, zadat regulární výraz tak, aby odpovídaly jeden nebo více názvů kolekcí (například collection01 | collection02 | collection03).</span><span class="sxs-lookup"><span data-stu-id="2fe1c-247">To import from multiple Azure Cosmos DB collections, provide a regular expression to match one or more collection names (e.g. collection01 | collection02 | collection03).</span></span> <span data-ttu-id="2fe1c-248">Může volitelně zadejte nebo zadejte soubor pro dotaz na filtr a tvar dat určených k importu.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-248">You may optionally specify, or provide a file for, a query to both filter and shape the data to be imported.</span></span>

> [!NOTE]
> <span data-ttu-id="2fe1c-249">Vzhledem k tomu, že kolekce pole přijímá regulární výrazy, při importu z jedné kolekce, jejichž název obsahuje regulární výraz znaků, musí tyto znaky uvozené odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-249">Since the collection field accepts regular expressions, if you are importing from a single collection whose name contains regular expression characters, then those characters must be escaped accordingly.</span></span>
> 
> 

<span data-ttu-id="2fe1c-250">Možnost program pro import zdroje Azure Cosmos DB má následující rozšířené možnosti:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-250">The Azure Cosmos DB source importer option has the following advanced options:</span></span>

1. <span data-ttu-id="2fe1c-251">Zahrnutí interní polí: Určuje, zda mají být zahrnuty vlastnosti dokumentu systému Azure Cosmos DB export (např. _rid, _ts).</span><span class="sxs-lookup"><span data-stu-id="2fe1c-251">Include Internal Fields: Specifies whether or not to include Azure Cosmos DB document system properties in the export (e.g. _rid, _ts).</span></span>
2. <span data-ttu-id="2fe1c-252">Počet opakovaných pokusů o selhání: Určuje počet opakovaných pokusů připojení k databázi Azure Cosmos v případě přechodných chyb (například přerušení připojení k síti).</span><span class="sxs-lookup"><span data-stu-id="2fe1c-252">Number of Retries on Failure: Specifies the number of times to retry the connection to Azure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
3. <span data-ttu-id="2fe1c-253">Interval opakování: Určuje dobu čekání mezi opakování pokusu o připojení k databázi Azure Cosmos v případě přechodných chyb (například přerušení připojení k síti).</span><span class="sxs-lookup"><span data-stu-id="2fe1c-253">Retry Interval: Specifies how long to wait between retrying the connection to Azure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
4. <span data-ttu-id="2fe1c-254">Režim připojení: Určuje režim připojení pro použití s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-254">Connection Mode: Specifies the connection mode to use with Azure Cosmos DB.</span></span> <span data-ttu-id="2fe1c-255">Dostupné možnosti jsou DirectTcp, DirectHttps a brány.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-255">The available choices are DirectTcp, DirectHttps, and Gateway.</span></span> <span data-ttu-id="2fe1c-256">Režimy přímé připojení je rychlejší, zatímco režimu brány je další brány firewall popisný jako pouze používá port 443.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-256">The direct connection modes are faster, while the gateway mode is more firewall friendly as it only uses port 443.</span></span>

![Snímek obrazovky databázi Cosmos Azure zdroj rozšířené možnosti](./media/import-data/documentdbsourceoptions.png)

> [!TIP]
> <span data-ttu-id="2fe1c-258">Nástroj pro import výchozí režim připojení DirectTcp.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-258">The import tool defaults to connection mode DirectTcp.</span></span> <span data-ttu-id="2fe1c-259">Pokud máte brány firewall problémy, přejděte do režimu připojení brány, jak vyžaduje jenom port 443.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-259">If you experience firewall issues, switch to connection mode Gateway, as it only requires port 443.</span></span>
> 
> 

<span data-ttu-id="2fe1c-260">Zde jsou některé ukázky příkazového řádku k importu z databáze Cosmos Azure:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-260">Here are some command line samples to import from Azure Cosmos DB:</span></span>

    #Migrate data from one Azure Cosmos DB collection to another Azure Cosmos DB collections
    dt.exe /s:CosmosDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:TEColl /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:TESessions /t.CollectionThroughput:2500

    #Migrate data from multiple Azure Cosmos DB collections to a single Azure Cosmos DB collection
    dt.exe /s:CosmosDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:comp1|comp2|comp3|comp4 /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:singleCollection /t.CollectionThroughput:2500

    #Export an Azure Cosmos DB collection to a JSON file
    dt.exe /s:CosmosDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:StoresSub /t:JsonFile /t.File:StoresExport.json /t.Overwrite /t.CollectionThroughput:2500

> [!TIP]
> <span data-ttu-id="2fe1c-261">Cosmos DB Data importovat nástroj Azure také podporuje import dat z [emulátoru DB Cosmos Azure](local-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="2fe1c-261">The Azure Cosmos DB Data Import Tool also supports import of data from the [Azure Cosmos DB Emulator](local-emulator.md).</span></span> <span data-ttu-id="2fe1c-262">Při importu dat z místní emulátoru, nastavte koncový bod na `https://localhost:<port>`.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-262">When importing data from a local emulator, set the endpoint to `https://localhost:<port>`.</span></span> 
> 
> 

## <span data-ttu-id="2fe1c-263"><a id="HBaseSource"></a>Chcete-li importovat z HBase</span><span class="sxs-lookup"><span data-stu-id="2fe1c-263"><a id="HBaseSource"></a>To import from HBase</span></span>
<span data-ttu-id="2fe1c-264">Možnost program pro import zdroje HBase umožňuje importovat data z tabulky HBase a volitelně data filtrovat.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-264">The HBase source importer option allows you to import data from an HBase table and optionally filter the data.</span></span> <span data-ttu-id="2fe1c-265">Tak, aby nastavení importu se co největší jsou k dispozici několik šablon.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-265">Several templates are provided so that setting up an import is as easy as possible.</span></span>

![Možnosti nastavení zdroje snímek HBase](./media/import-data/hbasesource1.png)

![Možnosti nastavení zdroje snímek HBase](./media/import-data/hbasesource2.png)

<span data-ttu-id="2fe1c-268">Formát HBase Stargate připojovacího řetězce je:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-268">The format of the HBase Stargate connection string is:</span></span>

    ServiceURL=<server-address>;Username=<username>;Password=<password>

> [!NOTE]
> <span data-ttu-id="2fe1c-269">Pomocí příkazu ověřte Ujistěte se, že zadané v poli instanci HBase je přístupná.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-269">Use the Verify command to ensure that the HBase instance specified in the connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="2fe1c-270">Zde je ukázka příkazového řádku k importu z HBase:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-270">Here is a command line sample to import from HBase:</span></span>

    dt.exe /s:HBase /s.ConnectionString:ServiceURL=<server-address>;Username=<username>;Password=<password> /s.Table:Contacts /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:hbaseimport

## <span data-ttu-id="2fe1c-271"><a id="DocumentDBBulkTarget"></a>Chcete-li importovat rozhraní API DocumentDB (hromadný Import)</span><span class="sxs-lookup"><span data-stu-id="2fe1c-271"><a id="DocumentDBBulkTarget"></a>To import to the DocumentDB API (Bulk Import)</span></span>
<span data-ttu-id="2fe1c-272">Program pro import hromadné DB Cosmos Azure umožňuje importovat z jakéhokoli z dostupných možností, pomocí Azure DB Cosmos uložené procedury pro efektivitu.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-272">The Azure Cosmos DB Bulk importer allows you to import from any of the available source options, using an Azure Cosmos DB stored procedure for efficiency.</span></span> <span data-ttu-id="2fe1c-273">Nástroj podporuje importovat do jedné kolekce rozdělena na oddíly jedním Azure Cosmos DB, a také horizontálně dělené importu, které je dat rozděleného mezi více kolekcí oddíly jedním Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-273">The tool supports import to one single-partitioned Azure Cosmos DB collection, as well as sharded import whereby data is partitioned across multiple single-partitioned Azure Cosmos DB collections.</span></span> <span data-ttu-id="2fe1c-274">Další informace o segmentace dat naleznete v tématu [dělení a škálování v Azure Cosmos DB](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="2fe1c-274">For more information about partitioning data, see [Partitioning and scaling in Azure Cosmos DB](partition-data.md).</span></span> <span data-ttu-id="2fe1c-275">Nástroj vytvořit, spuštění a pak odstraňte uložená procedura z kolekcí cíl.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-275">The tool will create, execute, and then delete the stored procedure from the target collection(s).</span></span>  

![Možnosti hromadné snímek databázi Cosmos Azure](./media/import-data/documentdbbulk.png)

<span data-ttu-id="2fe1c-277">Formát Azure Cosmos DB připojovacího řetězce je:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-277">The format of the Azure Cosmos DB connection string is:</span></span>

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

<span data-ttu-id="2fe1c-278">Připojovací řetězce k účtu Azure Cosmos DB lze načíst z okna klíče na portálu Azure, jak je popsáno v [Správa účtu Azure Cosmos DB](manage-account.md), ale název databáze musí být připojenou k připojení řetězec v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-278">The Azure Cosmos DB account connection string can be retrieved from the Keys blade of the Azure portal, as described in [How to manage an Azure Cosmos DB account](manage-account.md), however the name of the database needs to be appended to the connection string in the following format:</span></span>

    Database=<CosmosDB Database>;

> [!NOTE]
> <span data-ttu-id="2fe1c-279">Pomocí příkazu ověřte, zda je přístupná zadané v poli instanci Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-279">Use the Verify command to ensure that the Azure Cosmos DB instance specified in the connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="2fe1c-280">Chcete-li importovat na jedinou kolekci, zadejte název kolekce, do které data budou naimportovány a klikněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-280">To import to a single collection, enter the name of the collection to which data will be imported and click the Add button.</span></span> <span data-ttu-id="2fe1c-281">Import do více kolekcí, zadejte jednotlivé názvy kolekce jednotlivě nebo použijte následující syntaxi zadat více kolekcí: *collection_prefix*[index - end index počátečního].</span><span class="sxs-lookup"><span data-stu-id="2fe1c-281">To import to multiple collections, either enter each collection name individually or use the following syntax to specify multiple collections: *collection_prefix*[start index - end index].</span></span> <span data-ttu-id="2fe1c-282">Při zadávání více kolekcí prostřednictvím výše uvedené syntaxe, mějte následující:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-282">When specifying multiple collections via the aforementioned syntax, keep the following in mind:</span></span>

1. <span data-ttu-id="2fe1c-283">Jsou podporovány pouze celé číslo vzory názvů rozsahu.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-283">Only integer range name patterns are supported.</span></span> <span data-ttu-id="2fe1c-284">Například zadání kolekce [0-3] vytvoří následující kolekce: collection0, collection1, collection2, collection3.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-284">For example, specifying collection[0-3] will produce the following collections: collection0, collection1, collection2, collection3.</span></span>
2. <span data-ttu-id="2fe1c-285">Můžete použít zkrácené syntaxe: kolekce [3] se emitování stejnou sadu kolekcí uveden v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-285">You can use an abbreviated syntax: collection[3] will emit same set of collections mentioned in step 1.</span></span>
3. <span data-ttu-id="2fe1c-286">Lze zadat více než jeden nahrazení.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-286">More than one substitution can be provided.</span></span> <span data-ttu-id="2fe1c-287">Například kolekce [0-1] [0-9] vygeneruje 20 názvů kolekcí úvodními nulami (collection01,... 02... 03).</span><span class="sxs-lookup"><span data-stu-id="2fe1c-287">For example, collection[0-1] [0-9] will generate 20 collection names with leading zeros (collection01, ..02, ..03).</span></span>

<span data-ttu-id="2fe1c-288">Jakmile názvy kolekce byly nastaveny, vyberte požadované propustnost kolekcí (400 RUs na 10 000 RUs).</span><span class="sxs-lookup"><span data-stu-id="2fe1c-288">Once the collection name(s) have been specified, choose the desired throughput of the collection(s) (400 RUs to 10,000 RUs).</span></span> <span data-ttu-id="2fe1c-289">Pro nejlepší výkon import zvolte vyšší propustnost.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-289">For best import performance, choose a higher throughput.</span></span> <span data-ttu-id="2fe1c-290">Další informace o úrovně výkonu najdete v tématu [úrovně výkonu v Azure Cosmos DB](performance-levels.md).</span><span class="sxs-lookup"><span data-stu-id="2fe1c-290">For more information about performance levels, see [Performance levels in Azure Cosmos DB](performance-levels.md).</span></span>

> [!NOTE]
> <span data-ttu-id="2fe1c-291">Nastavení výkonu propustnost platí jenom pro vytvoření kolekce.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-291">The performance throughput setting only applies to collection creation.</span></span> <span data-ttu-id="2fe1c-292">Pokud existuje zadané kolekci, se nezmění jeho propustnost.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-292">If the specified collection already exists, its throughput will not be modified.</span></span>
> 
> 

<span data-ttu-id="2fe1c-293">Při importu do více kolekcí, na základě hash podporuje nástroj pro import horizontálního dělení.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-293">When importing to multiple collections, the import tool supports hash based sharding.</span></span> <span data-ttu-id="2fe1c-294">V tomto scénáři zadejte vlastnost dokumentu, které chcete použít jako klíč oddílu (Pokud klíč oddílu je prázdné, dokumenty bude horizontálně dělené náhodně mezi kolekcemi cíl).</span><span class="sxs-lookup"><span data-stu-id="2fe1c-294">In this scenario, specify the document property you wish to use as the Partition Key (if Partition Key is left blank, documents will be sharded randomly across the target collections).</span></span>

<span data-ttu-id="2fe1c-295">Můžete volitelně můžete určit, které pole ve zdroji import má být použit jako vlastnost id dokumentu Azure Cosmos DB během importu (Všimněte si, že pokud dokumenty neobsahují tuto vlastnost, pak nástroj pro import bude generovat identifikátor GUID jako hodnota vlastnosti id).</span><span class="sxs-lookup"><span data-stu-id="2fe1c-295">You may optionally specify which field in the import source should be used as the Azure Cosmos DB document id property during the import (note that if documents do not contain this property, then the import tool will generate a GUID as the id property value).</span></span>

<span data-ttu-id="2fe1c-296">Během importu nejsou k dispozici několik upřesňujících možností.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-296">There are a number of advanced options available during import.</span></span> <span data-ttu-id="2fe1c-297">Nejprve při zahrnuje nástroj výchozí hromadně importovat uložené procedury (BulkInsert.js), můžete zadat vlastní import uložené procedury:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-297">First, while the tool includes a default bulk import stored procedure (BulkInsert.js), you may choose to specify your own import stored procedure:</span></span>

 ![Snímek obrazovky databázi Cosmos Azure hromadné vložení sproc možnost](./media/import-data/bulkinsertsp.png)

<span data-ttu-id="2fe1c-299">Kromě toho při importu datum typy (například ze systému SQL Server nebo MongoDB), můžete zvolit tři možnosti importu:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-299">Additionally, when importing date types (e.g. from SQL Server or MongoDB), you can choose between three import options:</span></span>

 ![Snímek obrazovky databázi Cosmos Azure datum čas import možnosti](./media/import-data/datetimeoptions.png)

* <span data-ttu-id="2fe1c-301">Řetězec: Zachovat jako hodnotu řetězce</span><span class="sxs-lookup"><span data-stu-id="2fe1c-301">String: Persist as a string value</span></span>
* <span data-ttu-id="2fe1c-302">Epoch: Zachovat jako číslo hodnota Epoch</span><span class="sxs-lookup"><span data-stu-id="2fe1c-302">Epoch: Persist as an Epoch number value</span></span>
* <span data-ttu-id="2fe1c-303">Oba: Zachovat řetězec a Epoch číselné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-303">Both: Persist both string and Epoch number values.</span></span> <span data-ttu-id="2fe1c-304">Tato možnost vytvořit vnořený dokument, třeba: "date_joined": {"Value": "2013-10-21T21:17:25.2410000Z", "Epoch": 1382390245}</span><span class="sxs-lookup"><span data-stu-id="2fe1c-304">This option will create a subdocument, for example: "date_joined": { "Value": "2013-10-21T21:17:25.2410000Z", "Epoch": 1382390245 }</span></span>

<span data-ttu-id="2fe1c-305">Program pro import hromadné DB Cosmos Azure má následující další rozšířené možnosti:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-305">The Azure Cosmos DB Bulk importer has the following additional advanced options:</span></span>

1. <span data-ttu-id="2fe1c-306">Velikost dávky: Nástroj výchozí velikost dávky 50.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-306">Batch Size: The tool defaults to a batch size of 50.</span></span>  <span data-ttu-id="2fe1c-307">Pokud jsou dokumenty, které se importovat velké, zvažte, což snižuje velikost dávky.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-307">If the documents to be imported are large, consider lowering the batch size.</span></span> <span data-ttu-id="2fe1c-308">Pokud dokumenty, které se importovat malé, zvažte naopak vyvolání velikost dávky.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-308">Conversely, if the documents to be imported are small, consider raising the batch size.</span></span>
2. <span data-ttu-id="2fe1c-309">Skript maximální velikost (bajty): použije se výchozí hodnota nástroje skriptu maximální velikost 512KB</span><span class="sxs-lookup"><span data-stu-id="2fe1c-309">Max Script Size (bytes): The tool defaults to a max script size of 512KB</span></span>
3. <span data-ttu-id="2fe1c-310">Zakázat automatické generování Id: Pokud každému dokumentu určených k importu obsahuje id pole, pak výběrem této možnosti můžete zvýšit výkon.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-310">Disable Automatic Id Generation: If every document to be imported contains an id field, then selecting this option can increase performance.</span></span> <span data-ttu-id="2fe1c-311">Chybí hodnota v poli jedinečné id dokumenty nebudou importovány.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-311">Documents missing a unique id field will not be imported.</span></span>
4. <span data-ttu-id="2fe1c-312">Aktualizace stávající dokumenty: Nástroj výchozí není nahrazení stávající dokumenty docházet ke konfliktům id.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-312">Update Existing Documents: The tool defaults to not replacing existing documents with id conflicts.</span></span> <span data-ttu-id="2fe1c-313">Výběrem této možnosti umožníte přepsal stávající dokumenty s odpovídajícím ID.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-313">Selecting this option will allow overwriting existing documents with matching ids.</span></span> <span data-ttu-id="2fe1c-314">Tato funkce je užitečná pro naplánované data migrace, které aktualizovat existující dokumenty.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-314">This feature is useful for scheduled data migrations that update existing documents.</span></span>
5. <span data-ttu-id="2fe1c-315">Počet opakovaných pokusů o selhání: Určuje počet opakovaných pokusů připojení k databázi Azure Cosmos v případě přechodných chyb (například přerušení připojení k síti).</span><span class="sxs-lookup"><span data-stu-id="2fe1c-315">Number of Retries on Failure: Specifies the number of times to retry the connection to Azure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
6. <span data-ttu-id="2fe1c-316">Interval opakování: Určuje dobu čekání mezi opakování pokusu o připojení k databázi Azure Cosmos v případě přechodných chyb (například přerušení připojení k síti).</span><span class="sxs-lookup"><span data-stu-id="2fe1c-316">Retry Interval: Specifies how long to wait between retrying the connection to Azure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
7. <span data-ttu-id="2fe1c-317">Režim připojení: Určuje režim připojení pro použití s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-317">Connection Mode: Specifies the connection mode to use with Azure Cosmos DB.</span></span> <span data-ttu-id="2fe1c-318">Dostupné možnosti jsou DirectTcp, DirectHttps a brány.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-318">The available choices are DirectTcp, DirectHttps, and Gateway.</span></span> <span data-ttu-id="2fe1c-319">Režimy přímé připojení je rychlejší, zatímco režimu brány je další brány firewall popisný jako pouze používá port 443.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-319">The direct connection modes are faster, while the gateway mode is more firewall friendly as it only uses port 443.</span></span>

![Snímek obrazovky databázi Cosmos Azure hromadným importem rozšířené možnosti](./media/import-data/docdbbulkoptions.png)

> [!TIP]
> <span data-ttu-id="2fe1c-321">Nástroj pro import výchozí režim připojení DirectTcp.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-321">The import tool defaults to connection mode DirectTcp.</span></span> <span data-ttu-id="2fe1c-322">Pokud máte brány firewall problémy, přejděte do režimu připojení brány, jak vyžaduje jenom port 443.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-322">If you experience firewall issues, switch to connection mode Gateway, as it only requires port 443.</span></span>
> 
> 

## <span data-ttu-id="2fe1c-323"><a id="DocumentDBSeqTarget"></a>Chcete-li importovat rozhraní API DocumentDB (po sobě jdoucích Import záznamů)</span><span class="sxs-lookup"><span data-stu-id="2fe1c-323"><a id="DocumentDBSeqTarget"></a>To import to the DocumentDB API (Sequential Record Import)</span></span>
<span data-ttu-id="2fe1c-324">– Importér sekvenční záznam Azure Cosmos DB umožňuje importovat z jakéhokoli z dostupných možností na základě záznamu podle.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-324">The Azure Cosmos DB sequential record importer allows you to import from any of the available source options on a record by record basis.</span></span> <span data-ttu-id="2fe1c-325">Tuto možnost můžete zvolit, pokud při importu do existující kolekce, která dosáhla své kvóty uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-325">You might choose this option if you’re importing to an existing collection that has reached its quota of stored procedures.</span></span> <span data-ttu-id="2fe1c-326">Nástroj podporuje import do Azure Cosmos DB kolekce jedním (jedním oddílem i více oddílu), stejně jako horizontálně dělené importu, které jsou data rozdělena mezi více kolekcí Azure Cosmos DB jedním oddílem nebo více oddílů.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-326">The tool supports import to a single (both single-partition and multi-partition) Azure Cosmos DB collection, as well as sharded import whereby data is partitioned across multiple single-partition and/or multi-partition Azure Cosmos DB collections.</span></span> <span data-ttu-id="2fe1c-327">Další informace o segmentace dat naleznete v tématu [dělení a škálování v Azure Cosmos DB](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="2fe1c-327">For more information about partitioning data, see [Partitioning and scaling in Azure Cosmos DB](partition-data.md).</span></span>

![Snímek obrazovky databázi Cosmos Azure možnosti sekvenční záznamů importu](./media/import-data/documentdbsequential.png)

<span data-ttu-id="2fe1c-329">Formát Azure Cosmos DB připojovacího řetězce je:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-329">The format of the Azure Cosmos DB connection string is:</span></span>

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

<span data-ttu-id="2fe1c-330">Připojovací řetězce k účtu Azure Cosmos DB lze načíst z okna klíče na portálu Azure, jak je popsáno v [Správa účtu Azure Cosmos DB](manage-account.md), ale název databáze musí být připojenou k připojení řetězec v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-330">The Azure Cosmos DB account connection string can be retrieved from the Keys blade of the Azure portal, as described in [How to manage an Azure Cosmos DB account](manage-account.md), however the name of the database needs to be appended to the connection string in the following format:</span></span>

    Database=<Azure Cosmos DB Database>;

> [!NOTE]
> <span data-ttu-id="2fe1c-331">Pomocí příkazu ověřte, zda je přístupná zadané v poli instanci Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-331">Use the Verify command to ensure that the Azure Cosmos DB instance specified in the connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="2fe1c-332">Chcete-li importovat na jedinou kolekci, zadejte název kolekce, do které data budou naimportovány a klikněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-332">To import to a single collection, enter the name of the collection to which data will be imported and click the Add button.</span></span> <span data-ttu-id="2fe1c-333">Import do více kolekcí, zadejte jednotlivé názvy kolekce jednotlivě nebo použijte následující syntaxi zadat více kolekcí: *collection_prefix*[index - end index počátečního].</span><span class="sxs-lookup"><span data-stu-id="2fe1c-333">To import to multiple collections, either enter each collection name individually or use the following syntax to specify multiple collections: *collection_prefix*[start index - end index].</span></span> <span data-ttu-id="2fe1c-334">Při zadávání více kolekcí prostřednictvím výše uvedené syntaxe, mějte následující:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-334">When specifying multiple collections via the aforementioned syntax, keep the following in mind:</span></span>

1. <span data-ttu-id="2fe1c-335">Jsou podporovány pouze celé číslo vzory názvů rozsahu.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-335">Only integer range name patterns are supported.</span></span> <span data-ttu-id="2fe1c-336">Například zadání kolekce [0-3] vytvoří následující kolekce: collection0, collection1, collection2, collection3.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-336">For example, specifying collection[0-3] will produce the following collections: collection0, collection1, collection2, collection3.</span></span>
2. <span data-ttu-id="2fe1c-337">Můžete použít zkrácené syntaxe: kolekce [3] se emitování stejnou sadu kolekcí uveden v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-337">You can use an abbreviated syntax: collection[3] will emit same set of collections mentioned in step 1.</span></span>
3. <span data-ttu-id="2fe1c-338">Lze zadat více než jeden nahrazení.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-338">More than one substitution can be provided.</span></span> <span data-ttu-id="2fe1c-339">Například kolekce [0-1] [0-9] vygeneruje 20 názvů kolekcí úvodními nulami (collection01,... 02... 03).</span><span class="sxs-lookup"><span data-stu-id="2fe1c-339">For example, collection[0-1] [0-9] will generate 20 collection names with leading zeros (collection01, ..02, ..03).</span></span>

<span data-ttu-id="2fe1c-340">Jakmile názvy kolekce byly nastaveny, vyberte požadované propustnost kolekcí (400 RUs na 250 000 RUs).</span><span class="sxs-lookup"><span data-stu-id="2fe1c-340">Once the collection name(s) have been specified, choose the desired throughput of the collection(s) (400 RUs to 250,000 RUs).</span></span> <span data-ttu-id="2fe1c-341">Pro nejlepší výkon import zvolte vyšší propustnost.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-341">For best import performance, choose a higher throughput.</span></span> <span data-ttu-id="2fe1c-342">Další informace o úrovně výkonu najdete v tématu [úrovně výkonu v Azure Cosmos DB](performance-levels.md).</span><span class="sxs-lookup"><span data-stu-id="2fe1c-342">For more information about performance levels, see [Performance levels in Azure Cosmos DB](performance-levels.md).</span></span> <span data-ttu-id="2fe1c-343">Všechny import do kolekcí s propustností > 10 000 RUs bude vyžadovat klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-343">Any import to collections with throughput >10,000 RUs will require a partition key.</span></span> <span data-ttu-id="2fe1c-344">Pokud zvolíte možnost mít více než 250 000 RUs, musíte do souboru požadavek na portálu pro svůj účet vyšší.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-344">If you choose to have more than 250,000 RUs, you will need to file a request in the portal to have your account increased.</span></span>

> [!NOTE]
> <span data-ttu-id="2fe1c-345">Propustnost nastavení se vztahuje pouze k vytvoření kolekce.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-345">The throughput setting only applies to collection creation.</span></span> <span data-ttu-id="2fe1c-346">Pokud existuje zadané kolekci, se nezmění jeho propustnost.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-346">If the specified collection already exists, its throughput will not be modified.</span></span>
> 
> 

<span data-ttu-id="2fe1c-347">Při importu do více kolekcí, na základě hash podporuje nástroj pro import horizontálního dělení.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-347">When importing to multiple collections, the import tool supports hash based sharding.</span></span> <span data-ttu-id="2fe1c-348">V tomto scénáři zadejte vlastnost dokumentu, které chcete použít jako klíč oddílu (Pokud klíč oddílu je prázdné, dokumenty bude horizontálně dělené náhodně mezi kolekcemi cíl).</span><span class="sxs-lookup"><span data-stu-id="2fe1c-348">In this scenario, specify the document property you wish to use as the Partition Key (if Partition Key is left blank, documents will be sharded randomly across the target collections).</span></span>

<span data-ttu-id="2fe1c-349">Můžete volitelně můžete určit, které pole ve zdroji import má být použit jako vlastnost id dokumentu Azure Cosmos DB během importu (Všimněte si, že pokud dokumenty neobsahují tuto vlastnost, pak nástroj pro import bude generovat identifikátor GUID jako hodnota vlastnosti id).</span><span class="sxs-lookup"><span data-stu-id="2fe1c-349">You may optionally specify which field in the import source should be used as the Azure Cosmos DB document id property during the import (note that if documents do not contain this property, then the import tool will generate a GUID as the id property value).</span></span>

<span data-ttu-id="2fe1c-350">Během importu nejsou k dispozici několik upřesňujících možností.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-350">There are a number of advanced options available during import.</span></span> <span data-ttu-id="2fe1c-351">První při importu datum typy (například ze systému SQL Server nebo MongoDB), můžete zvolit tři možnosti importu:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-351">First, when importing date types (e.g. from SQL Server or MongoDB), you can choose between three import options:</span></span>

 ![Snímek obrazovky databázi Cosmos Azure datum čas import možnosti](./media/import-data/datetimeoptions.png)

* <span data-ttu-id="2fe1c-353">Řetězec: Zachovat jako hodnotu řetězce</span><span class="sxs-lookup"><span data-stu-id="2fe1c-353">String: Persist as a string value</span></span>
* <span data-ttu-id="2fe1c-354">Epoch: Zachovat jako číslo hodnota Epoch</span><span class="sxs-lookup"><span data-stu-id="2fe1c-354">Epoch: Persist as an Epoch number value</span></span>
* <span data-ttu-id="2fe1c-355">Oba: Zachovat řetězec a Epoch číselné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-355">Both: Persist both string and Epoch number values.</span></span> <span data-ttu-id="2fe1c-356">Tato možnost vytvořit vnořený dokument, třeba: "date_joined": {"Value": "2013-10-21T21:17:25.2410000Z", "Epoch": 1382390245}</span><span class="sxs-lookup"><span data-stu-id="2fe1c-356">This option will create a subdocument, for example: "date_joined": { "Value": "2013-10-21T21:17:25.2410000Z", "Epoch": 1382390245 }</span></span>

<span data-ttu-id="2fe1c-357">Azure Cosmos DB - program pro import po sobě jdoucích záznam obsahuje následující další pokročilé možnosti:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-357">The Azure Cosmos DB - Sequential record importer has the following additional advanced options:</span></span>

1. <span data-ttu-id="2fe1c-358">Počet požadavků na paralelní: nástroj výchozí nastavení je 2 paralelní požadavky.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-358">Number of Parallel Requests: The tool defaults to 2 parallel requests.</span></span> <span data-ttu-id="2fe1c-359">Dokumenty, které se importovat jsou malé, vezměte v úvahu vyvolání počet paralelní požadavků.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-359">If the documents to be imported are small, consider raising the number of parallel requests.</span></span> <span data-ttu-id="2fe1c-360">Všimněte si, že pokud toto číslo se vyvolá příliš mnoho, importu může dojít k omezení šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-360">Note that if this number is raised too much, the import may experience throttling.</span></span>
2. <span data-ttu-id="2fe1c-361">Zakázat automatické generování Id: Pokud každému dokumentu určených k importu obsahuje id pole, pak výběrem této možnosti můžete zvýšit výkon.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-361">Disable Automatic Id Generation: If every document to be imported contains an id field, then selecting this option can increase performance.</span></span> <span data-ttu-id="2fe1c-362">Chybí hodnota v poli jedinečné id dokumenty nebudou importovány.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-362">Documents missing a unique id field will not be imported.</span></span>
3. <span data-ttu-id="2fe1c-363">Aktualizace stávající dokumenty: Nástroj výchozí není nahrazení stávající dokumenty docházet ke konfliktům id.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-363">Update Existing Documents: The tool defaults to not replacing existing documents with id conflicts.</span></span> <span data-ttu-id="2fe1c-364">Výběrem této možnosti umožníte přepsal stávající dokumenty s odpovídajícím ID.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-364">Selecting this option will allow overwriting existing documents with matching ids.</span></span> <span data-ttu-id="2fe1c-365">Tato funkce je užitečná pro naplánované data migrace, které aktualizovat existující dokumenty.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-365">This feature is useful for scheduled data migrations that update existing documents.</span></span>
4. <span data-ttu-id="2fe1c-366">Počet opakovaných pokusů o selhání: Určuje počet opakovaných pokusů připojení k databázi Azure Cosmos v případě přechodných chyb (například přerušení připojení k síti).</span><span class="sxs-lookup"><span data-stu-id="2fe1c-366">Number of Retries on Failure: Specifies the number of times to retry the connection to Azure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
5. <span data-ttu-id="2fe1c-367">Interval opakování: Určuje dobu čekání mezi opakování pokusu o připojení k databázi Azure Cosmos v případě přechodných chyb (například přerušení připojení k síti).</span><span class="sxs-lookup"><span data-stu-id="2fe1c-367">Retry Interval: Specifies how long to wait between retrying the connection to Azure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
6. <span data-ttu-id="2fe1c-368">Režim připojení: Určuje režim připojení pro použití s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-368">Connection Mode: Specifies the connection mode to use with Azure Cosmos DB.</span></span> <span data-ttu-id="2fe1c-369">Dostupné možnosti jsou DirectTcp, DirectHttps a brány.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-369">The available choices are DirectTcp, DirectHttps, and Gateway.</span></span> <span data-ttu-id="2fe1c-370">Režimy přímé připojení je rychlejší, zatímco režimu brány je další brány firewall popisný jako pouze používá port 443.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-370">The direct connection modes are faster, while the gateway mode is more firewall friendly as it only uses port 443.</span></span>

![Snímek obrazovky databázi Cosmos Azure import sekvenční záznam rozšířené možnosti](./media/import-data/documentdbsequentialoptions.png)

> [!TIP]
> <span data-ttu-id="2fe1c-372">Nástroj pro import výchozí režim připojení DirectTcp.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-372">The import tool defaults to connection mode DirectTcp.</span></span> <span data-ttu-id="2fe1c-373">Pokud máte brány firewall problémy, přejděte do režimu připojení brány, jak vyžaduje jenom port 443.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-373">If you experience firewall issues, switch to connection mode Gateway, as it only requires port 443.</span></span>
> 
> 

## <span data-ttu-id="2fe1c-374"><a id="IndexingPolicy"></a>Při vytváření kolekcí Azure Cosmos DB zadat zásady indexování</span><span class="sxs-lookup"><span data-stu-id="2fe1c-374"><a id="IndexingPolicy"></a>Specify an indexing policy when creating Azure Cosmos DB collections</span></span>
<span data-ttu-id="2fe1c-375">Když povolíte nástroj pro migraci při vytváření kolekcí během importu, můžete zadat zásady indexování kolekcí.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-375">When you allow the migration tool to create collections during import, you can specify the indexing policy of the collections.</span></span> <span data-ttu-id="2fe1c-376">V části Rozšířené možnosti Azure DB Cosmos po sobě jdoucích záznamů možností a Azure Cosmos DB hromadného importu přejděte do části zásady indexování.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-376">In the advanced options section of the Azure Cosmos DB Bulk import and Azure Cosmos DB Sequential record options, navigate to the Indexing Policy section.</span></span>

![Snímek obrazovky Azure Cosmos DB indexování zásad rozšířené možnosti](./media/import-data/indexingpolicy1.png)

<span data-ttu-id="2fe1c-378">Pomocí zásady indexování rozšířené možnosti, můžete vybrat indexování soubor zásad, ručně zadat zásady indexování nebo vybrat ze sady výchozích šablon (kliknutím pravým tlačítkem do textového pole indexování zásady).</span><span class="sxs-lookup"><span data-stu-id="2fe1c-378">Using the Indexing Policy advanced option, you can select an indexing policy file, manually enter an indexing policy, or select from a set of default templates (by right clicking in the indexing policy textbox).</span></span>

<span data-ttu-id="2fe1c-379">Šablony zásad, které tento nástroj nabízí jsou:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-379">The policy templates the tool provides are:</span></span>

* <span data-ttu-id="2fe1c-380">Výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-380">Default.</span></span> <span data-ttu-id="2fe1c-381">Tato zásada je vhodné, když jste provádění dotazy na rovnost pro řetězce a pomocí klauzule ORDER BY, rozsah a dotazy na rovnost pro čísla.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-381">This policy is best when you’re performing equality queries against strings and using ORDER BY, range, and equality queries for numbers.</span></span> <span data-ttu-id="2fe1c-382">Tato zásada má nižší režijní náklady úložiště index než rozsah.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-382">This policy has a lower index storage overhead than Range.</span></span>
* <span data-ttu-id="2fe1c-383">Rozsah.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-383">Range.</span></span> <span data-ttu-id="2fe1c-384">Tato zásada je nejvhodnější, že používáte dotazy ORDER BY, rozsah a rovnost na čísla i řetězce.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-384">This policy is best you’re using ORDER BY, range and equality queries on both numbers and strings.</span></span> <span data-ttu-id="2fe1c-385">Tato zásada nemá vyšší index režijní náklady na úložiště než výchozí nebo hodnoty Hash.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-385">This policy has a higher index storage overhead than Default or Hash.</span></span>

![Snímek obrazovky Azure Cosmos DB indexování zásad rozšířené možnosti](./media/import-data/indexingpolicy2.png)

> [!NOTE]
> <span data-ttu-id="2fe1c-387">Pokud nezadáte zásady indexování, bude použita výchozí zásady.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-387">If you do not specify an indexing policy, then the default policy will be applied.</span></span> <span data-ttu-id="2fe1c-388">Další informace o indexování zásad najdete v tématu [Azure DB Cosmos indexování zásady](indexing-policies.md).</span><span class="sxs-lookup"><span data-stu-id="2fe1c-388">For more information about indexing policies, see [Azure Cosmos DB indexing policies](indexing-policies.md).</span></span>
> 
> 

## <a name="export-to-json-file"></a><span data-ttu-id="2fe1c-389">Exportovat do souboru JSON</span><span class="sxs-lookup"><span data-stu-id="2fe1c-389">Export to JSON file</span></span>
<span data-ttu-id="2fe1c-390">Exportu Azure Cosmos DB JSON umožňuje některé z dostupných možností exportovat do souboru JSON, který obsahuje pole dokumentů JSON.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-390">The Azure Cosmos DB JSON exporter allows you to export any of the available source options to a JSON file that contains an array of JSON documents.</span></span> <span data-ttu-id="2fe1c-391">Nástroj zpracuje exportu pro vás, nebo můžete k zobrazení výsledné příkaz migrace a spusťte příkaz.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-391">The tool will handle the export for you, or you can choose to view the resulting migration command and run the command yourself.</span></span> <span data-ttu-id="2fe1c-392">Výsledný soubor JSON může být uložená místně nebo v úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-392">The resulting JSON file may be stored locally or in Azure Blob storage.</span></span>

![Snímek obrazovky z Azure Cosmos DB JSON možností exportu místního souboru](./media/import-data/jsontarget.png)

![Možností exportu snímek z Azure Cosmos DB JSON Azure Blob storage](./media/import-data/jsontarget2.png)

<span data-ttu-id="2fe1c-395">Volitelně můžete prettify výsledný formát JSON, který zvýší velikost výsledného dokumentu při vytváření obsahu více číselné vyjádření.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-395">You may optionally choose to prettify the resulting JSON, which will increase the size of the resulting document while making the contents more human readable.</span></span>

    Standard JSON export
    [{"id":"Sample","Title":"About Paris","Language":{"Name":"English"},"Author":{"Name":"Don","Location":{"City":"Paris","Country":"France"}},"Content":"Don's document in Azure Cosmos DB is a valid JSON document as defined by the JSON spec.","PageViews":10000,"Topics":[{"Title":"History of Paris"},{"Title":"Places to see in Paris"}]}]

    Prettified JSON export
    [
     {
    "id": "Sample",
    "Title": "About Paris",
    "Language": {
      "Name": "English"
    },
    "Author": {
      "Name": "Don",
      "Location": {
        "City": "Paris",
        "Country": "France"
      }
    },
    "Content": "Don's document in Azure Cosmos DB is a valid JSON document as defined by the JSON spec.",
    "PageViews": 10000,
    "Topics": [
      {
        "Title": "History of Paris"
      },
      {
        "Title": "Places to see in Paris"
      }
    ]
    }]

## <a name="advanced-configuration"></a><span data-ttu-id="2fe1c-396">Pokročilá konfigurace</span><span class="sxs-lookup"><span data-stu-id="2fe1c-396">Advanced configuration</span></span>
<span data-ttu-id="2fe1c-397">V dialogovém okně Upřesnit konfiguraci zadejte umístění souboru protokolu, do kterého chcete všechny chyby zapsána.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-397">In the Advanced configuration screen, specify the location of the log file to which you would like any errors written.</span></span> <span data-ttu-id="2fe1c-398">Na této stránce platí následující pravidla:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-398">The following rules apply to this page:</span></span>

1. <span data-ttu-id="2fe1c-399">Pokud není zadaný název souboru, se na stránce výsledky vrátí všechny chyby.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-399">If a file name is not provided, then all errors will be returned on the Results page.</span></span>
2. <span data-ttu-id="2fe1c-400">Pokud je název souboru zadaný bez adresáře, pak soubor bude vytvořena (nebo přepsat) v aktuálním adresáři prostředí.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-400">If a file name is provided without a directory, then the file will be created (or overwritten) in the current environment directory.</span></span>
3. <span data-ttu-id="2fe1c-401">Pokud vyberete existující soubor, a soubor budou přepsány, není žádná možnost připojit.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-401">If you select an existing file, then the file will be overwritten, there is no append option.</span></span>

<span data-ttu-id="2fe1c-402">Potom vyberte, zda chcete protokolovat všechny, kritické, nebo žádná chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-402">Then, choose whether to log all, critical, or no error messages.</span></span> <span data-ttu-id="2fe1c-403">Nakonec rozhodnete, jak často budou aktualizovány na přenos zpráv obrazovky jejím průběhu.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-403">Finally, decide how frequently the on screen transfer message will be updated with its progress.</span></span>

    ![Screenshot of Advanced configuration screen](./media/import-data/AdvancedConfiguration.png)

## <a name="confirm-import-settings-and-view-command-line"></a><span data-ttu-id="2fe1c-404">Potvrďte nastavení pro import a zobrazení příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="2fe1c-404">Confirm import settings and view command line</span></span>
1. <span data-ttu-id="2fe1c-405">Po zadání informace o zdroji, informace o cílové a pokročilou konfiguraci, zkontrolujte souhrn migrace a, volitelně, zobrazení nebo zkopírování příkaz výsledné migrace (kopírování příkazu je užitečné k automatizaci operací import):</span><span class="sxs-lookup"><span data-stu-id="2fe1c-405">After specifying source information, target information, and advanced configuration, review the migration summary and, optionally, view/copy the resulting migration command (copying the command is useful to automate import operations):</span></span>
   
    ![Snímek obrazovky souhrn](./media/import-data/summary.png)
   
    ![Snímek obrazovky souhrn](./media/import-data/summarycommand.png)
2. <span data-ttu-id="2fe1c-408">Jakmile budete spokojeni s možnosti zdroje a cíle, klikněte na možnost **Import**.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-408">Once you’re satisfied with your source and target options, click **Import**.</span></span> <span data-ttu-id="2fe1c-409">Uplynulý čas, přenášená počet a informace o selhání (Pokud název souboru v pokročilé konfiguraci neposkytli) bude aktualizovat, protože proces importu.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-409">The elapsed time, transferred count, and failure information (if you didn't provide a file name in the Advanced configuration) will update as the import is in process.</span></span> <span data-ttu-id="2fe1c-410">Po dokončení můžete exportovat výsledky (například k řešení případných selhání import).</span><span class="sxs-lookup"><span data-stu-id="2fe1c-410">Once complete, you can export the results (e.g. to deal with any import failures).</span></span>
   
    ![Snímek obrazovky z Azure Cosmos DB JSON možností exportu](./media/import-data/viewresults.png)
3. <span data-ttu-id="2fe1c-412">Nový import, může spustit také zachovat stávající nastavení (například připojovací řetězec informace, zdroje a cíle volba atd.) nebo resetování všechny hodnoty.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-412">You may also start a new import, either keeping the existing settings (e.g. connection string information, source and target choice, etc.) or resetting all values.</span></span>
   
    ![Snímek obrazovky z Azure Cosmos DB JSON možností exportu](./media/import-data/newimport.png)

## <a name="next-steps"></a><span data-ttu-id="2fe1c-414">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2fe1c-414">Next steps</span></span>

<span data-ttu-id="2fe1c-415">V tomto kurzu jste provést následující:</span><span class="sxs-lookup"><span data-stu-id="2fe1c-415">In this tutorial, you've done the following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2fe1c-416">Nainstalovaný nástroj pro migraci dat</span><span class="sxs-lookup"><span data-stu-id="2fe1c-416">Installed the Data Migration tool</span></span>
> * <span data-ttu-id="2fe1c-417">Importovaná data z různých zdrojů dat.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-417">Imported data from different data sources</span></span>
> * <span data-ttu-id="2fe1c-418">Export z Azure Cosmos DB do formátu JSON</span><span class="sxs-lookup"><span data-stu-id="2fe1c-418">Exported from Azure Cosmos DB to JSON</span></span>

<span data-ttu-id="2fe1c-419">Teď můžete pokračovat v dalším kurzu a zjistěte, jak zadávat dotazy na data pomocí Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2fe1c-419">You can now proceed to the next tutorial and learn how to query data using Azure Cosmos DB.</span></span> 

> [!div class="nextstepaction"]
>[<span data-ttu-id="2fe1c-420">Postup dotazování dat?</span><span class="sxs-lookup"><span data-stu-id="2fe1c-420">How to query data?</span></span>](../cosmos-db/tutorial-query-documentdb.md)
