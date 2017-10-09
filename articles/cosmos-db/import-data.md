---
title: "Nástroj pro migraci aaaDatabase pro Azure Cosmos DB | Microsoft Docs"
description: "Zjistěte, jak toouse hello otevřete zdroj Azure Cosmos DB data migrace nástroje tooimport data tooAzure Cosmos DB z různých zdrojů, včetně souborů MongoDB, systému SQL Server, úložiště Table, Amazon DynamoDB, CSV a JSON. Převod tooJSON sdíleného svazku clusteru."
keywords: "CSV toojson, nástrojů pro migraci databáze, převést toojson sdíleného svazku clusteru"
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
ms.openlocfilehash: 997648a31602d854db75bb6ce4e2ecff36fc1069
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooimport-data-into-azure-cosmos-db-for-hello-documentdb-api"></a><span data-ttu-id="c5932-105">Jak hello tooimport data do Azure DB Cosmos pro rozhraní API DocumentDB?</span><span class="sxs-lookup"><span data-stu-id="c5932-105">How tooimport data into Azure Cosmos DB for hello DocumentDB API?</span></span>

<span data-ttu-id="c5932-106">Tento kurz obsahuje instrukce k používání hello Azure Cosmos DB: nástroj pro migraci dat rozhraní API DocumentDB, který můžete importovat data z různých zdrojů, včetně souborů JSON, CSV soubory, SQL, MongoDB, Azure Table storage, Amazon DynamoDB a Azure Cosmos databáze DocumentDB Rozhraní API kolekce do kolekce pro použití v Azure Cosmos DB a hello DocumentDB rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c5932-106">This tutorial provides instructions on using hello Azure Cosmos DB: DocumentDB API Data Migration tool, which can import data from various sources, including JSON files, CSV files, SQL, MongoDB, Azure Table storage, Amazon DynamoDB and Azure Cosmos DB DocumentDB API collections into collections for use with Azure Cosmos DB and hello DocumentDB API.</span></span> <span data-ttu-id="c5932-107">Nástroj pro migraci dat Hello mohou sloužit také při migraci z kolekce tvořené jedním oddílem kolekce tooa více oddílů pro hello DocumentDB rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c5932-107">hello Data Migration tool can also be used when migrating from a single partition collection tooa multi-partition collection for hello DocumentDB API.</span></span>

<span data-ttu-id="c5932-108">Nástroj pro migraci dat Hello funguje pouze při import dat do Azure DB Cosmos pro použití s hello DocumentDB rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c5932-108">hello Data Migration tool only works when importing data into Azure Cosmos DB for use with hello DocumentDB API.</span></span> <span data-ttu-id="c5932-109">V tuto chvíli není podporován import dat pro použití s hello tabulky API nebo rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="c5932-109">Importing data for use with hello Table API or Graph API is not supported at this time.</span></span> 

<span data-ttu-id="c5932-110">tooimport dat pro použití s hello MongoDB rozhraní API, najdete v části [Cosmos databázi Azure: jak hello toomigrate data pro rozhraní API MongoDB?](mongodb-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="c5932-110">tooimport data for use with hello MongoDB API, see [Azure Cosmos DB: How toomigrate data for hello MongoDB API?](mongodb-migrate.md).</span></span>

<span data-ttu-id="c5932-111">Tento kurz se zabývá hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="c5932-111">This tutorial covers hello following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c5932-112">Instalace nástroje pro migraci dat hello</span><span class="sxs-lookup"><span data-stu-id="c5932-112">Installing hello Data Migration tool</span></span>
> * <span data-ttu-id="c5932-113">Import dat z různých zdrojů dat.</span><span class="sxs-lookup"><span data-stu-id="c5932-113">Importing data from different data sources</span></span>
> * <span data-ttu-id="c5932-114">Export z Azure Cosmos DB tooJSON</span><span class="sxs-lookup"><span data-stu-id="c5932-114">Exporting from Azure Cosmos DB tooJSON</span></span>

## <span data-ttu-id="c5932-115"><a id="Prerequisites"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="c5932-115"><a id="Prerequisites"></a>Prerequisites</span></span>
<span data-ttu-id="c5932-116">Než budete postupovat hello pokyny v tomto článku, zajistěte, abyste měli hello nainstalované tyto položky:</span><span class="sxs-lookup"><span data-stu-id="c5932-116">Before following hello instructions in this article, ensure that you have hello following installed:</span></span>

* <span data-ttu-id="c5932-117">[Rozhraní Microsoft .NET Framework 4.51](https://www.microsoft.com/download/developer-tools.aspx) nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="c5932-117">[Microsoft .NET Framework 4.51](https://www.microsoft.com/download/developer-tools.aspx) or higher.</span></span>

## <span data-ttu-id="c5932-118"><a id="Overviewl"></a>Přehled nástroje pro migraci dat hello</span><span class="sxs-lookup"><span data-stu-id="c5932-118"><a id="Overviewl"></a>Overview of hello Data Migration tool</span></span>
<span data-ttu-id="c5932-119">Nástroj pro migraci dat Hello je řešení open source, který umožňuje importovat data tooAzure Cosmos DB z různých zdrojů, včetně:</span><span class="sxs-lookup"><span data-stu-id="c5932-119">hello Data Migration tool is an open source solution that imports data tooAzure Cosmos DB from a variety of sources, including:</span></span>

* <span data-ttu-id="c5932-120">Soubory JSON</span><span class="sxs-lookup"><span data-stu-id="c5932-120">JSON files</span></span>
* <span data-ttu-id="c5932-121">MongoDB</span><span class="sxs-lookup"><span data-stu-id="c5932-121">MongoDB</span></span>
* <span data-ttu-id="c5932-122">SQL Server</span><span class="sxs-lookup"><span data-stu-id="c5932-122">SQL Server</span></span>
* <span data-ttu-id="c5932-123">CSV soubory</span><span class="sxs-lookup"><span data-stu-id="c5932-123">CSV files</span></span>
* <span data-ttu-id="c5932-124">Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="c5932-124">Azure Table storage</span></span>
* <span data-ttu-id="c5932-125">Amazon DynamoDB</span><span class="sxs-lookup"><span data-stu-id="c5932-125">Amazon DynamoDB</span></span>
* <span data-ttu-id="c5932-126">HBase</span><span class="sxs-lookup"><span data-stu-id="c5932-126">HBase</span></span>
* <span data-ttu-id="c5932-127">Azure Cosmos DB kolekce</span><span class="sxs-lookup"><span data-stu-id="c5932-127">Azure Cosmos DB collections</span></span>

<span data-ttu-id="c5932-128">Když nástroj pro import hello zahrnuje grafické uživatelské rozhraní (dtui.exe), můžete také vycházejí z příkazového řádku hello (dt.exe).</span><span class="sxs-lookup"><span data-stu-id="c5932-128">While hello import tool includes a graphical user interface (dtui.exe), it can also be driven from hello command line (dt.exe).</span></span> <span data-ttu-id="c5932-129">Po nastavení importu prostřednictvím hello uživatelského rozhraní je ve skutečnosti příkaz hello přidružené toooutput možnost.</span><span class="sxs-lookup"><span data-stu-id="c5932-129">In fact, there is an option toooutput hello associated command after setting up an import through hello UI.</span></span> <span data-ttu-id="c5932-130">Tabulkové zdroje dat (např. SQL Server nebo CSV soubory) lze je transformovat tak, aby hierarchické vztahy (vnořené dokumenty) lze vytvořit při importu.</span><span class="sxs-lookup"><span data-stu-id="c5932-130">Tabular source data (e.g. SQL Server or CSV files) can be transformed such that hierarchical relationships (subdocuments) can be created during import.</span></span> <span data-ttu-id="c5932-131">Zachovat čtení toolearn informace o možnosti nastavení zdroje, tooimport příkazové řádky ukázkové z každého zdroje, target – možnosti a zobrazení pro import výsledků.</span><span class="sxs-lookup"><span data-stu-id="c5932-131">Keep reading toolearn more about source options, sample command lines tooimport from each source, target options, and viewing import results.</span></span>

## <span data-ttu-id="c5932-132"><a id="Install"></a>Nainstalujte nástroj pro migraci dat hello</span><span class="sxs-lookup"><span data-stu-id="c5932-132"><a id="Install"></a>Install hello Data Migration tool</span></span>
<span data-ttu-id="c5932-133">Hello migrace nástroj zdrojový kód je k dispozici na Githubu v [toto úložiště](https://github.com/azure/azure-documentdb-datamigrationtool) a je k dispozici z kompilované verze [Microsoft Download Center](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d).</span><span class="sxs-lookup"><span data-stu-id="c5932-133">hello migration tool source code is available on GitHub in [this repository](https://github.com/azure/azure-documentdb-datamigrationtool) and a compiled version is available from [Microsoft Download Center](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d).</span></span> <span data-ttu-id="c5932-134">Může buď zkompilovat hello řešení nebo jednoduše stažení a extrakci hello zkompilovaná tooa adresáři podle vašeho výběru.</span><span class="sxs-lookup"><span data-stu-id="c5932-134">You may either compile hello solution or simply download and extract hello compiled version tooa directory of your choice.</span></span> <span data-ttu-id="c5932-135">Spusťte buď:</span><span class="sxs-lookup"><span data-stu-id="c5932-135">Then run either:</span></span>

* <span data-ttu-id="c5932-136">**Dtui.exe**: verze grafického rozhraní nástroje hello</span><span class="sxs-lookup"><span data-stu-id="c5932-136">**Dtui.exe**: Graphical interface version of hello tool</span></span>
* <span data-ttu-id="c5932-137">**DT.exe**: příkazového řádku verze nástroje hello</span><span class="sxs-lookup"><span data-stu-id="c5932-137">**Dt.exe**: Command-line version of hello tool</span></span>

## <a name="import-data"></a><span data-ttu-id="c5932-138">Import dat</span><span class="sxs-lookup"><span data-stu-id="c5932-138">Import data</span></span>

<span data-ttu-id="c5932-139">Jakmile jste nainstalovali nástroj hello, je čas tooimport vaše data.</span><span class="sxs-lookup"><span data-stu-id="c5932-139">Once you've installed hello tool, it's time tooimport your data.</span></span> <span data-ttu-id="c5932-140">Jaký typ dat chcete tooimport?</span><span class="sxs-lookup"><span data-stu-id="c5932-140">What kind of data do you want tooimport?</span></span>

* [<span data-ttu-id="c5932-141">Soubory JSON</span><span class="sxs-lookup"><span data-stu-id="c5932-141">JSON files</span></span>](#JSON)
* [<span data-ttu-id="c5932-142">MongoDB</span><span class="sxs-lookup"><span data-stu-id="c5932-142">MongoDB</span></span>](#MongoDB)
* [<span data-ttu-id="c5932-143">MongoDB exportní soubory</span><span class="sxs-lookup"><span data-stu-id="c5932-143">MongoDB Export files</span></span>](#MongoDBExport)
* [<span data-ttu-id="c5932-144">SQL Server</span><span class="sxs-lookup"><span data-stu-id="c5932-144">SQL Server</span></span>](#SQL)
* [<span data-ttu-id="c5932-145">CSV soubory</span><span class="sxs-lookup"><span data-stu-id="c5932-145">CSV files</span></span>](#CSV)
* [<span data-ttu-id="c5932-146">Azure Table storage</span><span class="sxs-lookup"><span data-stu-id="c5932-146">Azure Table storage</span></span>](#AzureTableSource)
* [<span data-ttu-id="c5932-147">Amazon DynamoDB</span><span class="sxs-lookup"><span data-stu-id="c5932-147">Amazon DynamoDB</span></span>](#DynamoDBSource)
* [<span data-ttu-id="c5932-148">Objekt BLOB</span><span class="sxs-lookup"><span data-stu-id="c5932-148">Blob</span></span>](#BlobImport)
* [<span data-ttu-id="c5932-149">Azure Cosmos DB kolekce</span><span class="sxs-lookup"><span data-stu-id="c5932-149">Azure Cosmos DB collections</span></span>](#DocumentDBSource)
* [<span data-ttu-id="c5932-150">HBase</span><span class="sxs-lookup"><span data-stu-id="c5932-150">HBase</span></span>](#HBaseSource)
* [<span data-ttu-id="c5932-151">Azure Cosmos DB hromadného importu</span><span class="sxs-lookup"><span data-stu-id="c5932-151">Azure Cosmos DB bulk import</span></span>](#DocumentDBBulkImport)
* [<span data-ttu-id="c5932-152">Sekvenční záznam importu do Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="c5932-152">Azure Cosmos DB sequential record import</span></span>](#DocumentDSeqTarget)


## <span data-ttu-id="c5932-153"><a id="JSON"></a>soubory JSON tooimport</span><span class="sxs-lookup"><span data-stu-id="c5932-153"><a id="JSON"></a>tooimport JSON files</span></span>
<span data-ttu-id="c5932-154">Hello možnost program pro import zdrojového souboru JSON umožňuje tooimport jeden nebo více jednotlivý dokument JSON soubory nebo soubory JSON, že každý obsahovat pole dokumentů JSON.</span><span class="sxs-lookup"><span data-stu-id="c5932-154">hello JSON file source importer option allows you tooimport one or more single document JSON files or JSON files that each contain an array of JSON documents.</span></span> <span data-ttu-id="c5932-155">Při přidávání složek, které obsahují tooimport soubory JSON, máte možnost hello rekurzivní hledání souborů v podsložkách.</span><span class="sxs-lookup"><span data-stu-id="c5932-155">When adding folders that contain JSON files tooimport, you have hello option of recursively searching for files in subfolders.</span></span>

![Možnosti nastavení zdroje souboru snímek JSON - nástrojů pro migraci databáze](./media/import-data/jsonsource.png)

<span data-ttu-id="c5932-157">Zde jsou některé soubory JSON tooimport ukázky příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="c5932-157">Here are some command line samples tooimport JSON files:</span></span>

    #Import a single JSON file
    dt.exe /s:JsonFile /s.Files:.\Sessions.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Sessions /t.CollectionThroughput:2500

    #Import a directory of JSON files
    dt.exe /s:JsonFile /s.Files:C:\TESessions\*.json /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Sessions /t.CollectionThroughput:2500

    #Import a directory (including sub-directories) of JSON files
    dt.exe /s:JsonFile /s.Files:C:\LastFMMusic\**\*.json /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Music /t.CollectionThroughput:2500

    #Import a directory (single), directory (recursive), and individual JSON files
    dt.exe /s:JsonFile /s.Files:C:\Tweets\*.*;C:\LargeDocs\**\*.*;C:\TESessions\Session48172.json;C:\TESessions\Session48173.json;C:\TESessions\Session48174.json;C:\TESessions\Session48175.json;C:\TESessions\Session48177.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:subs /t.CollectionThroughput:2500

    #Import a single JSON file and partition hello data across 4 collections
    dt.exe /s:JsonFile /s.Files:D:\\CompanyData\\Companies.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:comp[1-4] /t.PartitionKey:name /t.CollectionThroughput:2500

## <span data-ttu-id="c5932-158"><a id="MongoDB"></a>tooimport z MongoDB</span><span class="sxs-lookup"><span data-stu-id="c5932-158"><a id="MongoDB"></a>tooimport from MongoDB</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c5932-159">Pokud importujete účet Azure Cosmos DB tooan s podporou pro MongoDB, postupujte podle těchto [pokyny](mongodb-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="c5932-159">If you are importing tooan Azure Cosmos DB account with Support for MongoDB, follow these [instructions](mongodb-migrate.md).</span></span>
> 
> 

<span data-ttu-id="c5932-160">Hello MongoDB zdrojový program pro import možnost vám umožní tooimport z kolekci jednotlivých MongoDB a volitelně filtrovat dokumentů pomocí dotazu nebo změna struktury dokumentu hello pomocí projekce.</span><span class="sxs-lookup"><span data-stu-id="c5932-160">hello MongoDB source importer option allows you tooimport from an individual MongoDB collection and optionally filter documents using a query and/or modify hello document structure by using a projection.</span></span>  

![Možnosti nastavení zdroje snímek MongoDB](./media/import-data/mongodbsource.png)

<span data-ttu-id="c5932-162">Hello připojovací řetězec je ve formátu MongoDB standard hello:</span><span class="sxs-lookup"><span data-stu-id="c5932-162">hello connection string is in hello standard MongoDB format:</span></span>

    mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database>

> [!NOTE]
> <span data-ttu-id="c5932-163">Je možné přistupovat pomocí hello ověřte, zda příkaz tooensure, který hello zadané v poli řetězec připojení hello instanci MongoDB.</span><span class="sxs-lookup"><span data-stu-id="c5932-163">Use hello Verify command tooensure that hello MongoDB instance specified in hello connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="c5932-164">Zadejte název hello hello kolekce, ze kterého budou importována data.</span><span class="sxs-lookup"><span data-stu-id="c5932-164">Enter hello name of hello collection from which data will be imported.</span></span> <span data-ttu-id="c5932-165">Může volitelně zadejte nebo zadejte soubor pro dotaz (například {pop: {$gt: 5000}}) nebo projekce (například {loc:0}) tooboth filtru a tvar hello data toobe importovat.</span><span class="sxs-lookup"><span data-stu-id="c5932-165">You may optionally specify or provide a file for a query (e.g. {pop: {$gt:5000}} ) and/or projection (e.g. {loc:0} ) tooboth filter and shape hello data toobe imported.</span></span>

<span data-ttu-id="c5932-166">Zde jsou některé tooimport ukázky příkazový řádek z MongoDB:</span><span class="sxs-lookup"><span data-stu-id="c5932-166">Here are some command line samples tooimport from MongoDB:</span></span>

    #Import all documents from a MongoDB collection
    dt.exe /s:MongoDB /s.ConnectionString:mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database> /s.Collection:zips /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:BulkZips /t.IdField:_id /t.CollectionThroughput:2500

    #Import documents from a MongoDB collection which match hello query and exclude hello loc field
    dt.exe /s:MongoDB /s.ConnectionString:mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database> /s.Collection:zips /s.Query:{pop:{$gt:50000}} /s.Projection:{loc:0} /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:BulkZipsTransform /t.IdField:_id/t.CollectionThroughput:2500

## <span data-ttu-id="c5932-167"><a id="MongoDBExport"></a>tooimport MongoDB exportní soubory</span><span class="sxs-lookup"><span data-stu-id="c5932-167"><a id="MongoDBExport"></a>tooimport MongoDB export files</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c5932-168">Pokud importujete účet Azure Cosmos DB tooan s podporou pro MongoDB, postupujte podle těchto [pokyny](mongodb-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="c5932-168">If you are importing tooan Azure Cosmos DB account with support for MongoDB, follow these [instructions](mongodb-migrate.md).</span></span>
> 
> 

<span data-ttu-id="c5932-169">Hello MongoDB export JSON souboru zdrojový program pro import možnost vám umožní tooimport jeden nebo více soubory JSON vytvořeného z hello mongoexport nástroj.</span><span class="sxs-lookup"><span data-stu-id="c5932-169">hello MongoDB export JSON file source importer option allows you tooimport one or more JSON files produced from hello mongoexport utility.</span></span>  

![Možnosti nastavení zdroje export snímek MongoDB](./media/import-data/mongodbexportsource.png)

<span data-ttu-id="c5932-171">Při přidávání složek, které obsahují soubory JSON export MongoDB pro import, máte možnost hello rekurzivní hledání souborů v podsložkách.</span><span class="sxs-lookup"><span data-stu-id="c5932-171">When adding folders that contain MongoDB export JSON files for import, you have hello option of recursively searching for files in subfolders.</span></span>

<span data-ttu-id="c5932-172">Tady je příkazový řádek ukázkové tooimport MongoDB exportu soubory JSON:</span><span class="sxs-lookup"><span data-stu-id="c5932-172">Here is a command line sample tooimport from MongoDB export JSON files:</span></span>

    dt.exe /s:MongoDBExport /s.Files:D:\mongoemployees.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:employees /t.IdField:_id /t.Dates:Epoch /t.CollectionThroughput:2500

## <span data-ttu-id="c5932-173"><a id="SQL"></a>tooimport z SQL serveru</span><span class="sxs-lookup"><span data-stu-id="c5932-173"><a id="SQL"></a>tooimport from SQL Server</span></span>
<span data-ttu-id="c5932-174">Hello možnost program pro import zdroje SQL můžete tooimport z jednotlivých databáze systému SQL Server a volitelně filtrování toobe záznamy hello importován pomocí dotazu.</span><span class="sxs-lookup"><span data-stu-id="c5932-174">hello SQL source importer option allows you tooimport from an individual SQL Server database and optionally filter hello records toobe imported using a query.</span></span> <span data-ttu-id="c5932-175">Kromě toho můžete změnit strukturu dokumentu hello zadáním vnoření oddělovače (podrobnosti o této za chvíli).</span><span class="sxs-lookup"><span data-stu-id="c5932-175">In addition, you can modify hello document structure by specifying a nesting separator (more on that in a moment).</span></span>  

![Možnosti nastavení zdroje snímek SQL – nástroje pro migraci databáze](./media/import-data/sqlexportsource.png)

<span data-ttu-id="c5932-177">Formát Hello hello připojovacího řetězce je formátu řetězce připojení standardní SQL hello.</span><span class="sxs-lookup"><span data-stu-id="c5932-177">hello format of hello connection string is hello standard SQL connection string format.</span></span>

> [!NOTE]
> <span data-ttu-id="c5932-178">Použití hello ověřte, zda příkaz tooensure, který hello zadané v poli řetězec připojení hello instanci serveru SQL je přístupná.</span><span class="sxs-lookup"><span data-stu-id="c5932-178">Use hello Verify command tooensure that hello SQL Server instance specified in hello connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="c5932-179">Hello vnoření vlastnost oddělovače je použité toocreate hierarchické vztahy (dílčí dokumenty) během importu.</span><span class="sxs-lookup"><span data-stu-id="c5932-179">hello nesting separator property is used toocreate hierarchical relationships (sub-documents) during import.</span></span> <span data-ttu-id="c5932-180">Vezměte v úvahu hello následující dotazu SQL:</span><span class="sxs-lookup"><span data-stu-id="c5932-180">Consider hello following SQL query:</span></span>

<span data-ttu-id="c5932-181">*jako Id, názvu, AddressType jako [Address.AddressType], AddressLine1 jako [Address.AddressLine1], města jako [Address.Location.City], StateProvinceName jako [Address.Location.StateProvinceName], PostalCode jako [[[vyberte PŘETYPOVÁNÍ (BusinessEntityID AS varchar) Address.PostalCode] CountryRegionName jako [Address.CountryRegionName] z Sales.vStoreWithAddresses kde AddressType = "hlavní kanceláře.*</span><span class="sxs-lookup"><span data-stu-id="c5932-181">*select CAST(BusinessEntityID AS varchar) as Id, Name, AddressType as [Address.AddressType], AddressLine1 as [Address.AddressLine1], City as [Address.Location.City], StateProvinceName as [Address.Location.StateProvinceName], PostalCode as [Address.PostalCode], CountryRegionName as [Address.CountryRegionName] from Sales.vStoreWithAddresses WHERE AddressType='Main Office'*</span></span>

<span data-ttu-id="c5932-182">Která vrací hello následující výsledky (částečné):</span><span class="sxs-lookup"><span data-stu-id="c5932-182">Which returns hello following (partial) results:</span></span>

![Snímek obrazovky SQL výsledky dotazu](./media/import-data/sqlqueryresults.png)

<span data-ttu-id="c5932-184">Poznámka: aliasy hello například Address.AddressType a Address.Location.StateProvinceName.</span><span class="sxs-lookup"><span data-stu-id="c5932-184">Note hello aliases such as Address.AddressType and Address.Location.StateProvinceName.</span></span> <span data-ttu-id="c5932-185">Zadáním vnoření oddělovač '.', nástroj pro import hello vytvoří adres a import vnořené dokumenty Address.Location během hello.</span><span class="sxs-lookup"><span data-stu-id="c5932-185">By specifying a nesting separator of ‘.’, hello import tool creates Address and Address.Location subdocuments during hello import.</span></span> <span data-ttu-id="c5932-186">Tady je příklad výsledné dokumentu v Azure Cosmos DB:</span><span class="sxs-lookup"><span data-stu-id="c5932-186">Here is an example of a resulting document in Azure Cosmos DB:</span></span>

<span data-ttu-id="c5932-187">*{"id": "956", "název": "Jemnějšího prodeje a služba", "Adres": {"AddressType": "Hlavní Office", "AddressLine1": "#500 75 O'Connor ulici", "Umístění": {"City": "Ottawa", "StateProvinceName": "Ontario"}, "PostalCode": "K4B 1S2", "CountryRegionName": " Kanada"}}*</span><span class="sxs-lookup"><span data-stu-id="c5932-187">*{ "id": "956", "Name": "Finer Sales and Service", "Address": { "AddressType": "Main Office", "AddressLine1": "#500-75 O'Connor Street", "Location": { "City": "Ottawa", "StateProvinceName": "Ontario" }, "PostalCode": "K4B 1S2", "CountryRegionName": "Canada" } }*</span></span>

<span data-ttu-id="c5932-188">Zde jsou některé tooimport ukázky příkazový řádek z SQL serveru:</span><span class="sxs-lookup"><span data-stu-id="c5932-188">Here are some command line samples tooimport from SQL Server:</span></span>

    #Import records from SQL which match a query
    dt.exe /s:SQL /s.ConnectionString:"Data Source=<server>;Initial Catalog=AdventureWorks;User Id=advworks;Password=<password>;" /s.Query:"select CAST(BusinessEntityID AS varchar) as Id, * from Sales.vStoreWithAddresses WHERE AddressType='Main Office'" /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Stores /t.IdField:Id /t.CollectionThroughput:2500

    #Import records from sql which match a query and create hierarchical relationships
    dt.exe /s:SQL /s.ConnectionString:"Data Source=<server>;Initial Catalog=AdventureWorks;User Id=advworks;Password=<password>;" /s.Query:"select CAST(BusinessEntityID AS varchar) as Id, Name, AddressType as [Address.AddressType], AddressLine1 as [Address.AddressLine1], City as [Address.Location.City], StateProvinceName as [Address.Location.StateProvinceName], PostalCode as [Address.PostalCode], CountryRegionName as [Address.CountryRegionName] from Sales.vStoreWithAddresses WHERE AddressType='Main Office'" /s.NestingSeparator:. /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:StoresSub /t.IdField:Id /t.CollectionThroughput:2500

## <span data-ttu-id="c5932-189"><a id="CSV"></a>CSV soubory tooimport a tooJSON převést sdíleného svazku clusteru</span><span class="sxs-lookup"><span data-stu-id="c5932-189"><a id="CSV"></a>tooimport CSV files and convert CSV tooJSON</span></span>
<span data-ttu-id="c5932-190">Hello možnost program pro import zdrojového souboru CSV umožňuje tooimport je jeden nebo více souborů CSV.</span><span class="sxs-lookup"><span data-stu-id="c5932-190">hello CSV file source importer option enables you tooimport one or more CSV files.</span></span> <span data-ttu-id="c5932-191">Při přidávání složek, které obsahují soubory sdíleného svazku clusteru pro import, máte možnost hello rekurzivní hledání souborů v podsložkách.</span><span class="sxs-lookup"><span data-stu-id="c5932-191">When adding folders that contain CSV files for import, you have hello option of recursively searching for files in subfolders.</span></span>

![Možnosti nastavení zdroje snímek CSV - tooJSON sdíleného svazku clusteru](media/import-data/csvsource.png)

<span data-ttu-id="c5932-193">Podobné toohello SQL zdroje, hello vnoření oddělovače vlastnost může být použité toocreate hierarchické vztahy (dílčí dokumenty) během importu.</span><span class="sxs-lookup"><span data-stu-id="c5932-193">Similar toohello SQL source, hello nesting separator property may be used toocreate hierarchical relationships (sub-documents) during import.</span></span> <span data-ttu-id="c5932-194">Vezměte v úvahu hello následující řádek záhlaví sdíleného svazku clusteru a řádků dat:</span><span class="sxs-lookup"><span data-stu-id="c5932-194">Consider hello following CSV header row and data rows:</span></span>

![Snímek obrazovky CSV ukázkové záznamy - tooJSON sdíleného svazku clusteru](./media/import-data/csvsample.png)

<span data-ttu-id="c5932-196">Poznámka: aliasy hello například DomainInfo.Domain_Name a RedirectInfo.Redirecting.</span><span class="sxs-lookup"><span data-stu-id="c5932-196">Note hello aliases such as DomainInfo.Domain_Name and RedirectInfo.Redirecting.</span></span> <span data-ttu-id="c5932-197">Zadáním vnoření oddělovač '.', vytvoří nástroj pro import hello DomainInfo a vnořené dokumenty RedirectInfo během hello importovat.</span><span class="sxs-lookup"><span data-stu-id="c5932-197">By specifying a nesting separator of ‘.’, hello import tool will create DomainInfo and RedirectInfo subdocuments during hello import.</span></span> <span data-ttu-id="c5932-198">Tady je příklad výsledné dokumentu v Azure Cosmos DB:</span><span class="sxs-lookup"><span data-stu-id="c5932-198">Here is an example of a resulting document in Azure Cosmos DB:</span></span>

<span data-ttu-id="c5932-199">*{"DomainInfo": {"Domain_Name": "ACUS.GOV", "Domain_Name_Address": "http://www. ACUS.GOV"},"Federal agenturou":"Správu konferencí hello USA","RedirectInfo": {"Přesměrování":"0","Redirect_Destination":" "},"id":"9cc565c5-ebcd-1c03-ebd3-cc3e2ecd814d"}*</span><span class="sxs-lookup"><span data-stu-id="c5932-199">*{ "DomainInfo": { "Domain_Name": "ACUS.GOV", "Domain_Name_Address": "http://www.ACUS.GOV" }, "Federal Agency": "Administrative Conference of hello United States", "RedirectInfo": { "Redirecting": "0", "Redirect_Destination": "" }, "id": "9cc565c5-ebcd-1c03-ebd3-cc3e2ecd814d" }*</span></span>

<span data-ttu-id="c5932-200">Nástroj pro import Hello pokusí tooinfer informací o typu nekotovaných hodnoty v souborů CSV (hodnoty v uvozovkách se vždy pracuje jako řetězce).</span><span class="sxs-lookup"><span data-stu-id="c5932-200">hello import tool will attempt tooinfer type information for unquoted values in CSV files (quoted values are always treated as strings).</span></span>  <span data-ttu-id="c5932-201">Typy jsou označeny v následujícím pořadí hello: číslo, datum a čas, logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="c5932-201">Types are identified in hello following order: number, datetime, boolean.</span></span>  

<span data-ttu-id="c5932-202">Existují dvě jiných věcí toonote o importu souboru CSV:</span><span class="sxs-lookup"><span data-stu-id="c5932-202">There are two other things toonote about CSV import:</span></span>

1. <span data-ttu-id="c5932-203">Ve výchozím nastavení, jsou nekotovaných hodnoty vždy oříznut pro karty a prostory, zatímco se zachovají hodnoty v uvozovkách jako-je.</span><span class="sxs-lookup"><span data-stu-id="c5932-203">By default, unquoted values are always trimmed for tabs and spaces, while quoted values are preserved as-is.</span></span> <span data-ttu-id="c5932-204">Toto chování lze přepsat pomocí hello Trim v uvozovkách hodnoty zaškrtávací políčko nebo hello /s.TrimQuoted možnosti příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="c5932-204">This behavior can be overridden with hello Trim quoted values checkbox or hello /s.TrimQuoted command line option.</span></span>
2. <span data-ttu-id="c5932-205">Ve výchozím nastavení nekotovaných null považován za hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="c5932-205">By default, an unquoted null is treated as a null value.</span></span> <span data-ttu-id="c5932-206">Toto chování můžete přepsat (tj. považovat za nekotovaných null řetězec "null") s hello zpracovávat nekotované NULL jako řetězec zaškrtávací políčko nebo hello /s.NoUnquotedNulls možnosti příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="c5932-206">This behavior can be overridden (i.e. treat an unquoted null as a “null” string) with hello Treat unquoted NULL as string checkbox or hello /s.NoUnquotedNulls command line option.</span></span>

<span data-ttu-id="c5932-207">Zde je ukázka příkazového řádku pro importu souboru CSV:</span><span class="sxs-lookup"><span data-stu-id="c5932-207">Here is a command line sample for CSV import:</span></span>

    dt.exe /s:CsvFile /s.Files:.\Employees.csv /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Employees /t.IdField:EntityID /t.CollectionThroughput:2500

## <span data-ttu-id="c5932-208"><a id="AzureTableSource"></a>tooimport z úložiště tabulek Azure</span><span class="sxs-lookup"><span data-stu-id="c5932-208"><a id="AzureTableSource"></a>tooimport from Azure Table storage</span></span>
<span data-ttu-id="c5932-209">Hello možnost program pro import zdroje úložiště Azure Table vám umožní tooimport z jednotlivé tabulky Azure Table storage a volitelně filtrovat hello tabulka entity toobe importovat.</span><span class="sxs-lookup"><span data-stu-id="c5932-209">hello Azure Table storage source importer option allows you tooimport from an individual Azure Table storage table and optionally filter hello table entities toobe imported.</span></span> <span data-ttu-id="c5932-210">Upozorňujeme, že nelze použít hello migrace dat nástroj tooimport Azure Table storage data do Azure Cosmos DB pro použití s hello tabulky rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c5932-210">Note that you cannot use hello Data Migration tool tooimport Azure Table storage data into Azure Cosmos DB for use with hello Table API.</span></span> <span data-ttu-id="c5932-211">V tuto chvíli je podporována pouze import tooAzure Cosmos DB pro použití s hello DocumentDB rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c5932-211">Only importing tooAzure Cosmos DB for use with hello DocumentDB API is supported at this time.</span></span>

![Možnosti nastavení zdroje snímek Azure Table storage](./media/import-data/azuretablesource.png)

<span data-ttu-id="c5932-213">Formát Hello hello Azure Table storage připojovacího řetězce je:</span><span class="sxs-lookup"><span data-stu-id="c5932-213">hello format of hello Azure Table storage connection string is:</span></span>

    DefaultEndpointsProtocol=<protocol>;AccountName=<Account Name>;AccountKey=<Account Key>;

> [!NOTE]
> <span data-ttu-id="c5932-214">Použití hello ověřte, zda příkaz tooensure, který hello zadané v pole řetězce připojení hello instanci Azure Table storage je přístupná.</span><span class="sxs-lookup"><span data-stu-id="c5932-214">Use hello Verify command tooensure that hello Azure Table storage instance specified in hello connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="c5932-215">Zadejte název hello hello Azure tabulka, ze kterého budou importována data.</span><span class="sxs-lookup"><span data-stu-id="c5932-215">Enter hello name of hello Azure table from which data will be imported.</span></span> <span data-ttu-id="c5932-216">Volitelně můžete určit [filtru](https://msdn.microsoft.com/library/azure/ff683669.aspx).</span><span class="sxs-lookup"><span data-stu-id="c5932-216">You may optionally specify a [filter](https://msdn.microsoft.com/library/azure/ff683669.aspx).</span></span>

<span data-ttu-id="c5932-217">Hello možnost program pro import zdroje úložiště Azure Table má hello následující další možnosti:</span><span class="sxs-lookup"><span data-stu-id="c5932-217">hello Azure Table storage source importer option has hello following additional options:</span></span>

1. <span data-ttu-id="c5932-218">Zahrnutí interní polí</span><span class="sxs-lookup"><span data-stu-id="c5932-218">Include Internal Fields</span></span>
   1. <span data-ttu-id="c5932-219">Všechny - obsahovat všechny interní pole (klíč oddílu, RowKey a časové razítko)</span><span class="sxs-lookup"><span data-stu-id="c5932-219">All - Include all internal fields (PartitionKey, RowKey, and Timestamp)</span></span>
   2. <span data-ttu-id="c5932-220">Žádný - vyloučit všechny interní pole</span><span class="sxs-lookup"><span data-stu-id="c5932-220">None - Exclude all internal fields</span></span>
   3. <span data-ttu-id="c5932-221">RowKey – obsahovat pouze pole RowKey hello</span><span class="sxs-lookup"><span data-stu-id="c5932-221">RowKey - Only include hello RowKey field</span></span>
2. <span data-ttu-id="c5932-222">Vyberte sloupce</span><span class="sxs-lookup"><span data-stu-id="c5932-222">Select Columns</span></span>
   1. <span data-ttu-id="c5932-223">Azure Table storage filtry nepodporují projekce.</span><span class="sxs-lookup"><span data-stu-id="c5932-223">Azure Table storage filters do not support projections.</span></span> <span data-ttu-id="c5932-224">Pokud chcete tooonly import specifické Azure Table entity vlastnosti, přidáte toohello výběr sloupců seznamu.</span><span class="sxs-lookup"><span data-stu-id="c5932-224">If you want tooonly import specific Azure Table entity properties, add them toohello Select Columns list.</span></span> <span data-ttu-id="c5932-225">Všechny ostatní vlastnosti entity, bude ignorována.</span><span class="sxs-lookup"><span data-stu-id="c5932-225">All other entity properties will be ignored.</span></span>

<span data-ttu-id="c5932-226">Tady je tooimport ukázka příkazový řádek z úložiště tabulek Azure:</span><span class="sxs-lookup"><span data-stu-id="c5932-226">Here is a command line sample tooimport from Azure Table storage:</span></span>

    dt.exe /s:AzureTable /s.ConnectionString:"DefaultEndpointsProtocol=https;AccountName=<Account Name>;AccountKey=<Account Key>" /s.Table:metrics /s.InternalFields:All /s.Filter:"PartitionKey eq 'Partition1' and RowKey gt '00001'" /s.Projection:ObjectCount;ObjectSize  /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:metrics /t.CollectionThroughput:2500

## <span data-ttu-id="c5932-227"><a id="DynamoDBSource"></a>tooimport z Amazon DynamoDB</span><span class="sxs-lookup"><span data-stu-id="c5932-227"><a id="DynamoDBSource"></a>tooimport from Amazon DynamoDB</span></span>
<span data-ttu-id="c5932-228">Hello Amazon DynamoDB zdrojový program pro import možnost vám umožní tooimport z jednotlivé tabulky Amazon DynamoDB a volitelně filtrování toobe entity hello importovat.</span><span class="sxs-lookup"><span data-stu-id="c5932-228">hello Amazon DynamoDB source importer option allows you tooimport from an individual Amazon DynamoDB table and optionally filter hello entities toobe imported.</span></span> <span data-ttu-id="c5932-229">Tak, aby nastavení importu se co největší jsou k dispozici několik šablon.</span><span class="sxs-lookup"><span data-stu-id="c5932-229">Several templates are provided so that setting up an import is as easy as possible.</span></span>

![Možnosti nastavení zdroje DynamoDB Amazon – snímek obrazovky - nástrojů pro migraci databáze](./media/import-data/dynamodbsource1.png)

![Možnosti nastavení zdroje DynamoDB Amazon – snímek obrazovky - nástrojů pro migraci databáze](./media/import-data/dynamodbsource2.png)

<span data-ttu-id="c5932-232">Formát Hello hello Amazon DynamoDB připojovacího řetězce je:</span><span class="sxs-lookup"><span data-stu-id="c5932-232">hello format of hello Amazon DynamoDB connection string is:</span></span>

    ServiceURL=<Service Address>;AccessKey=<Access Key>;SecretKey=<Secret Key>;

> [!NOTE]
> <span data-ttu-id="c5932-233">Použití hello ověřte, zda příkaz tooensure, který hello zadané v poli řetězec připojení hello instanci Amazon DynamoDB je přístupná.</span><span class="sxs-lookup"><span data-stu-id="c5932-233">Use hello Verify command tooensure that hello Amazon DynamoDB instance specified in hello connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="c5932-234">Tady je tooimport ukázka příkazový řádek z Amazon DynamoDB:</span><span class="sxs-lookup"><span data-stu-id="c5932-234">Here is a command line sample tooimport from Amazon DynamoDB:</span></span>

    dt.exe /s:DynamoDB /s.ConnectionString:ServiceURL=https://dynamodb.us-east-1.amazonaws.com;AccessKey=<accessKey>;SecretKey=<secretKey> /s.Request:"{   """TableName""": """ProductCatalog""" }" /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<Azure Cosmos DB Endpoint>;AccountKey=<Azure Cosmos DB Key>;Database=<Azure Cosmos DB Database>;" /t.Collection:catalogCollection /t.CollectionThroughput:2500

## <span data-ttu-id="c5932-235"><a id="BlobImport"></a>soubory tooimport z Azure Blob storage</span><span class="sxs-lookup"><span data-stu-id="c5932-235"><a id="BlobImport"></a>tooimport files from Azure Blob storage</span></span>
<span data-ttu-id="c5932-236">Hello soubor JSON, MongoDB export souboru a možnosti program pro import zdrojového souboru CSV umožňují tooimport jeden nebo více souborů z úložiště objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="c5932-236">hello JSON file, MongoDB export file, and CSV file source importer options allow you tooimport one or more files from Azure Blob storage.</span></span> <span data-ttu-id="c5932-237">Po zadání adresy URL kontejneru objektu Blob a klíč účtu, stačí zadejte tooimport soubory hello tooselect regulárním výrazem.</span><span class="sxs-lookup"><span data-stu-id="c5932-237">After specifying a Blob container URL and Account Key, simply provide a regular expression tooselect hello file(s) tooimport.</span></span>

![Možnosti nastavení zdroje snímek objektu Blob souboru](./media/import-data/blobsource.png)

<span data-ttu-id="c5932-239">Tady je příkazového řádku ukázkové tooimport JSON soubory z Azure Blob storage:</span><span class="sxs-lookup"><span data-stu-id="c5932-239">Here is command line sample tooimport JSON files from Azure Blob storage:</span></span>

    dt.exe /s:JsonFile /s.Files:"blobs://<account key>@account.blob.core.windows.net:443/importcontainer/.*" /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:doctest

## <span data-ttu-id="c5932-240"><a id="DocumentDBSource"></a>tooimport z kolekce rozhraní API služby Azure Cosmos databáze DocumentDB</span><span class="sxs-lookup"><span data-stu-id="c5932-240"><a id="DocumentDBSource"></a>tooimport from an Azure Cosmos DB DocumentDB API collection</span></span>
<span data-ttu-id="c5932-241">Hello možnost program pro import zdroje Azure Cosmos DB vám umožní tooimport data z jedné nebo více kolekcí Azure Cosmos DB a volitelně filtrování dokumentů pomocí dotazu.</span><span class="sxs-lookup"><span data-stu-id="c5932-241">hello Azure Cosmos DB source importer option allows you tooimport data from one or more Azure Cosmos DB collections and optionally filter documents using a query.</span></span>  

![Možnosti nastavení zdroje snímek databázi Cosmos Azure](./media/import-data/documentdbsource.png)

<span data-ttu-id="c5932-243">Formát Hello hello Azure Cosmos DB připojovacího řetězce je:</span><span class="sxs-lookup"><span data-stu-id="c5932-243">hello format of hello Azure Cosmos DB connection string is:</span></span>

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

<span data-ttu-id="c5932-244">Hello připojovací řetězce k účtu Azure Cosmos DB lze načíst z okna klíče hello hello portál Azure, jak je popsáno v [jak toomanage účet Azure Cosmos DB](manage-account.md), ale název hello hello databáze musí toohello toobe připojeno připojovací řetězec v hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="c5932-244">hello Azure Cosmos DB account connection string can be retrieved from hello Keys blade of hello Azure portal, as described in [How toomanage an Azure Cosmos DB account](manage-account.md), however hello name of hello database needs toobe appended toohello connection string in hello following format:</span></span>

    Database=<CosmosDB Database>;

> [!NOTE]
> <span data-ttu-id="c5932-245">Použití hello ověřte, zda příkaz tooensure, který hello zadané v poli řetězec připojení hello instanci Azure Cosmos databáze je přístupná.</span><span class="sxs-lookup"><span data-stu-id="c5932-245">Use hello Verify command tooensure that hello Azure Cosmos DB instance specified in hello connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="c5932-246">tooimport z jedné kolekce Azure Cosmos DB, zadejte název hello hello kolekce, ze kterého budou importována data.</span><span class="sxs-lookup"><span data-stu-id="c5932-246">tooimport from a single Azure Cosmos DB collection, enter hello name of hello collection from which data will be imported.</span></span> <span data-ttu-id="c5932-247">tooimport z více kolekcí Azure Cosmos DB, jeden nebo více názvy kolekce poskytují toomatch regulární výraz (například collection01 | collection02 | collection03).</span><span class="sxs-lookup"><span data-stu-id="c5932-247">tooimport from multiple Azure Cosmos DB collections, provide a regular expression toomatch one or more collection names (e.g. collection01 | collection02 | collection03).</span></span> <span data-ttu-id="c5932-248">Může volitelně můžete zadat, nebo zadejte soubor pro tooboth filtru dotazu a utvářejí toobe hello data importovat.</span><span class="sxs-lookup"><span data-stu-id="c5932-248">You may optionally specify, or provide a file for, a query tooboth filter and shape hello data toobe imported.</span></span>

> [!NOTE]
> <span data-ttu-id="c5932-249">Vzhledem k tomu, že hello kolekce pole přijímá regulární výrazy, při importu z jedné kolekce, jejichž název obsahuje regulární výraz znaků, musí tyto znaky uvozené odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="c5932-249">Since hello collection field accepts regular expressions, if you are importing from a single collection whose name contains regular expression characters, then those characters must be escaped accordingly.</span></span>
> 
> 

<span data-ttu-id="c5932-250">Hello Azure Cosmos DB zdrojový program pro import možnost má následující hello rozšířené možnosti:</span><span class="sxs-lookup"><span data-stu-id="c5932-250">hello Azure Cosmos DB source importer option has hello following advanced options:</span></span>

1. <span data-ttu-id="c5932-251">Zahrnutí interní polí: Určuje, zda tooinclude Azure Cosmos DB dokumentu systému vlastnosti v hello exportovat (například _rid, _ts).</span><span class="sxs-lookup"><span data-stu-id="c5932-251">Include Internal Fields: Specifies whether or not tooinclude Azure Cosmos DB document system properties in hello export (e.g. _rid, _ts).</span></span>
2. <span data-ttu-id="c5932-252">Počet opakovaných pokusů o selhání: Určuje hello počet tooretry hello připojení tooAzure Cosmos DB v případě přechodných chyb (například přerušení připojení k síti).</span><span class="sxs-lookup"><span data-stu-id="c5932-252">Number of Retries on Failure: Specifies hello number of times tooretry hello connection tooAzure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
3. <span data-ttu-id="c5932-253">Interval opakování: Určuje, jak dlouho toowait mezi opakování hello připojení tooAzure Cosmos DB v případě přechodných chyb (například přerušení připojení k síti).</span><span class="sxs-lookup"><span data-stu-id="c5932-253">Retry Interval: Specifies how long toowait between retrying hello connection tooAzure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
4. <span data-ttu-id="c5932-254">Režim připojení: Určuje hello připojení režimu toouse s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="c5932-254">Connection Mode: Specifies hello connection mode toouse with Azure Cosmos DB.</span></span> <span data-ttu-id="c5932-255">jsou k dispozici možnosti Hello DirectTcp, DirectHttps a brány.</span><span class="sxs-lookup"><span data-stu-id="c5932-255">hello available choices are DirectTcp, DirectHttps, and Gateway.</span></span> <span data-ttu-id="c5932-256">režimy Hello přímé připojení jsou rychlejší, zatímco hello brány režim je další brány firewall popisný jako pouze používá port 443.</span><span class="sxs-lookup"><span data-stu-id="c5932-256">hello direct connection modes are faster, while hello gateway mode is more firewall friendly as it only uses port 443.</span></span>

![Snímek obrazovky databázi Cosmos Azure zdroj rozšířené možnosti](./media/import-data/documentdbsourceoptions.png)

> [!TIP]
> <span data-ttu-id="c5932-258">Nástroj výchozí tooconnection režim DirectTcp import Hello.</span><span class="sxs-lookup"><span data-stu-id="c5932-258">hello import tool defaults tooconnection mode DirectTcp.</span></span> <span data-ttu-id="c5932-259">Pokud máte brány firewall problémy, přepněte tooconnection režimu bránu, vyžaduje jenom port 443.</span><span class="sxs-lookup"><span data-stu-id="c5932-259">If you experience firewall issues, switch tooconnection mode Gateway, as it only requires port 443.</span></span>
> 
> 

<span data-ttu-id="c5932-260">Zde jsou některé tooimport ukázky příkazový řádek z databáze Cosmos Azure:</span><span class="sxs-lookup"><span data-stu-id="c5932-260">Here are some command line samples tooimport from Azure Cosmos DB:</span></span>

    #Migrate data from one Azure Cosmos DB collection tooanother Azure Cosmos DB collections
    dt.exe /s:CosmosDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:TEColl /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:TESessions /t.CollectionThroughput:2500

    #Migrate data from multiple Azure Cosmos DB collections tooa single Azure Cosmos DB collection
    dt.exe /s:CosmosDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:comp1|comp2|comp3|comp4 /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:singleCollection /t.CollectionThroughput:2500

    #Export an Azure Cosmos DB collection tooa JSON file
    dt.exe /s:CosmosDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:StoresSub /t:JsonFile /t.File:StoresExport.json /t.Overwrite /t.CollectionThroughput:2500

> [!TIP]
> <span data-ttu-id="c5932-261">Hello Azure Cosmos DB dat nástroj pro Import také podporuje import dat z hello [emulátoru DB Cosmos Azure](local-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="c5932-261">hello Azure Cosmos DB Data Import Tool also supports import of data from hello [Azure Cosmos DB Emulator](local-emulator.md).</span></span> <span data-ttu-id="c5932-262">Při importu dat z místní emulátoru, nastavte koncový bod hello příliš`https://localhost:<port>`.</span><span class="sxs-lookup"><span data-stu-id="c5932-262">When importing data from a local emulator, set hello endpoint too`https://localhost:<port>`.</span></span> 
> 
> 

## <span data-ttu-id="c5932-263"><a id="HBaseSource"></a>tooimport z HBase</span><span class="sxs-lookup"><span data-stu-id="c5932-263"><a id="HBaseSource"></a>tooimport from HBase</span></span>
<span data-ttu-id="c5932-264">Hello možnost program pro import zdroje HBase můžete tooimport data z tabulky HBase a volitelně filtrování dat hello.</span><span class="sxs-lookup"><span data-stu-id="c5932-264">hello HBase source importer option allows you tooimport data from an HBase table and optionally filter hello data.</span></span> <span data-ttu-id="c5932-265">Tak, aby nastavení importu se co největší jsou k dispozici několik šablon.</span><span class="sxs-lookup"><span data-stu-id="c5932-265">Several templates are provided so that setting up an import is as easy as possible.</span></span>

![Možnosti nastavení zdroje snímek HBase](./media/import-data/hbasesource1.png)

![Možnosti nastavení zdroje snímek HBase](./media/import-data/hbasesource2.png)

<span data-ttu-id="c5932-268">Formát Hello hello HBase Stargate připojovacího řetězce je:</span><span class="sxs-lookup"><span data-stu-id="c5932-268">hello format of hello HBase Stargate connection string is:</span></span>

    ServiceURL=<server-address>;Username=<username>;Password=<password>

> [!NOTE]
> <span data-ttu-id="c5932-269">Použití hello ověřte, zda příkaz tooensure, který hello zadané v poli řetězec připojení hello instanci HBase je přístupná.</span><span class="sxs-lookup"><span data-stu-id="c5932-269">Use hello Verify command tooensure that hello HBase instance specified in hello connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="c5932-270">Tady je tooimport ukázka příkazový řádek z HBase:</span><span class="sxs-lookup"><span data-stu-id="c5932-270">Here is a command line sample tooimport from HBase:</span></span>

    dt.exe /s:HBase /s.ConnectionString:ServiceURL=<server-address>;Username=<username>;Password=<password> /s.Table:Contacts /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:hbaseimport

## <span data-ttu-id="c5932-271"><a id="DocumentDBBulkTarget"></a>tooimport toohello DocumentDB rozhraní API (hromadným importem)</span><span class="sxs-lookup"><span data-stu-id="c5932-271"><a id="DocumentDBBulkTarget"></a>tooimport toohello DocumentDB API (Bulk Import)</span></span>
<span data-ttu-id="c5932-272">Program pro import Azure Cosmos DB hromadné Hello umožňuje tooimport ze všech dostupných možností hello pomocí Azure DB Cosmos uložené procedury pro efektivitu.</span><span class="sxs-lookup"><span data-stu-id="c5932-272">hello Azure Cosmos DB Bulk importer allows you tooimport from any of hello available source options, using an Azure Cosmos DB stored procedure for efficiency.</span></span> <span data-ttu-id="c5932-273">Nástroj Hello podporuje import tooone oddíly jedním Azure Cosmos DB kolekce, jakož i horizontálně dělené importu, které je dat rozděleného mezi více kolekcí oddíly jedním Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="c5932-273">hello tool supports import tooone single-partitioned Azure Cosmos DB collection, as well as sharded import whereby data is partitioned across multiple single-partitioned Azure Cosmos DB collections.</span></span> <span data-ttu-id="c5932-274">Další informace o segmentace dat naleznete v tématu [dělení a škálování v Azure Cosmos DB](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="c5932-274">For more information about partitioning data, see [Partitioning and scaling in Azure Cosmos DB](partition-data.md).</span></span> <span data-ttu-id="c5932-275">Nástroj Hello bude vytvořte, spuštění a potom odstraňte hello uložené procedury z kolekcí cíl hello.</span><span class="sxs-lookup"><span data-stu-id="c5932-275">hello tool will create, execute, and then delete hello stored procedure from hello target collection(s).</span></span>  

![Možnosti hromadné snímek databázi Cosmos Azure](./media/import-data/documentdbbulk.png)

<span data-ttu-id="c5932-277">Formát Hello hello Azure Cosmos DB připojovacího řetězce je:</span><span class="sxs-lookup"><span data-stu-id="c5932-277">hello format of hello Azure Cosmos DB connection string is:</span></span>

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

<span data-ttu-id="c5932-278">Hello připojovací řetězce k účtu Azure Cosmos DB lze načíst z okna klíče hello hello portál Azure, jak je popsáno v [jak toomanage účet Azure Cosmos DB](manage-account.md), ale název hello hello databáze musí toohello toobe připojeno připojovací řetězec v hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="c5932-278">hello Azure Cosmos DB account connection string can be retrieved from hello Keys blade of hello Azure portal, as described in [How toomanage an Azure Cosmos DB account](manage-account.md), however hello name of hello database needs toobe appended toohello connection string in hello following format:</span></span>

    Database=<CosmosDB Database>;

> [!NOTE]
> <span data-ttu-id="c5932-279">Použití hello ověřte, zda příkaz tooensure, který hello zadané v poli řetězec připojení hello instanci Azure Cosmos databáze je přístupná.</span><span class="sxs-lookup"><span data-stu-id="c5932-279">Use hello Verify command tooensure that hello Azure Cosmos DB instance specified in hello connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="c5932-280">tooimport tooa jedna kolekce, zadejte název hello hello kolekce toowhich data budou naimportovány a klikněte na tlačítko Přidat hello.</span><span class="sxs-lookup"><span data-stu-id="c5932-280">tooimport tooa single collection, enter hello name of hello collection toowhich data will be imported and click hello Add button.</span></span> <span data-ttu-id="c5932-281">tooimport toomultiple kolekcí, zadejte jednotlivé názvy kolekce jednotlivě nebo použijte následující syntaxi toospecify hello více kolekcí: *collection_prefix*[index - end index počátečního].</span><span class="sxs-lookup"><span data-stu-id="c5932-281">tooimport toomultiple collections, either enter each collection name individually or use hello following syntax toospecify multiple collections: *collection_prefix*[start index - end index].</span></span> <span data-ttu-id="c5932-282">Při zadávání více kolekcí prostřednictvím hello výše uvedené syntaxe, mějte hello následující skutečnosti:</span><span class="sxs-lookup"><span data-stu-id="c5932-282">When specifying multiple collections via hello aforementioned syntax, keep hello following in mind:</span></span>

1. <span data-ttu-id="c5932-283">Jsou podporovány pouze celé číslo vzory názvů rozsahu.</span><span class="sxs-lookup"><span data-stu-id="c5932-283">Only integer range name patterns are supported.</span></span> <span data-ttu-id="c5932-284">Například zadání kolekce [0-3] vytvoří hello následující kolekce: collection0, collection1, collection2, collection3.</span><span class="sxs-lookup"><span data-stu-id="c5932-284">For example, specifying collection[0-3] will produce hello following collections: collection0, collection1, collection2, collection3.</span></span>
2. <span data-ttu-id="c5932-285">Můžete použít zkrácené syntaxe: kolekce [3] se emitování stejnou sadu kolekcí uveden v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="c5932-285">You can use an abbreviated syntax: collection[3] will emit same set of collections mentioned in step 1.</span></span>
3. <span data-ttu-id="c5932-286">Lze zadat více než jeden nahrazení.</span><span class="sxs-lookup"><span data-stu-id="c5932-286">More than one substitution can be provided.</span></span> <span data-ttu-id="c5932-287">Například kolekce [0-1] [0-9] vygeneruje 20 názvů kolekcí úvodními nulami (collection01,... 02... 03).</span><span class="sxs-lookup"><span data-stu-id="c5932-287">For example, collection[0-1] [0-9] will generate 20 collection names with leading zeros (collection01, ..02, ..03).</span></span>

<span data-ttu-id="c5932-288">Jakmile hello kolekce názvy byly nastaveny, vyberte požadované propustnost hello hello kolekcí (400 RUs too10, 000 RUs).</span><span class="sxs-lookup"><span data-stu-id="c5932-288">Once hello collection name(s) have been specified, choose hello desired throughput of hello collection(s) (400 RUs too10,000 RUs).</span></span> <span data-ttu-id="c5932-289">Pro nejlepší výkon import zvolte vyšší propustnost.</span><span class="sxs-lookup"><span data-stu-id="c5932-289">For best import performance, choose a higher throughput.</span></span> <span data-ttu-id="c5932-290">Další informace o úrovně výkonu najdete v tématu [úrovně výkonu v Azure Cosmos DB](performance-levels.md).</span><span class="sxs-lookup"><span data-stu-id="c5932-290">For more information about performance levels, see [Performance levels in Azure Cosmos DB](performance-levels.md).</span></span>

> [!NOTE]
> <span data-ttu-id="c5932-291">Hello výkonu propustnost nastavení se vztahuje pouze toocollection vytvoření.</span><span class="sxs-lookup"><span data-stu-id="c5932-291">hello performance throughput setting only applies toocollection creation.</span></span> <span data-ttu-id="c5932-292">Pokud hello zadané kolekce už existuje, se nezmění jeho propustnost.</span><span class="sxs-lookup"><span data-stu-id="c5932-292">If hello specified collection already exists, its throughput will not be modified.</span></span>
> 
> 

<span data-ttu-id="c5932-293">Při importu toomultiple kolekce, nástroj pro import hello podporuje hodnoty hash na základě horizontálního dělení.</span><span class="sxs-lookup"><span data-stu-id="c5932-293">When importing toomultiple collections, hello import tool supports hash based sharding.</span></span> <span data-ttu-id="c5932-294">V tomto scénáři, určete vlastnost dokumentu hello chcete toouse jako klíč oddílu hello (Pokud klíč oddílu je prázdné, dokumenty bude horizontálně dělené náhodně napříč hello cílové kolekce).</span><span class="sxs-lookup"><span data-stu-id="c5932-294">In this scenario, specify hello document property you wish toouse as hello Partition Key (if Partition Key is left blank, documents will be sharded randomly across hello target collections).</span></span>

<span data-ttu-id="c5932-295">Můžete volitelně můžete určit, které pole ve zdroji import hello má být použit jako hello vlastnost id dokumentu Azure Cosmos DB během importu hello (Všimněte si, že pokud dokumenty neobsahují tuto vlastnost, pak nástroj pro import hello bude generovat identifikátor GUID jako hodnota vlastnosti id hello).</span><span class="sxs-lookup"><span data-stu-id="c5932-295">You may optionally specify which field in hello import source should be used as hello Azure Cosmos DB document id property during hello import (note that if documents do not contain this property, then hello import tool will generate a GUID as hello id property value).</span></span>

<span data-ttu-id="c5932-296">Během importu nejsou k dispozici několik upřesňujících možností.</span><span class="sxs-lookup"><span data-stu-id="c5932-296">There are a number of advanced options available during import.</span></span> <span data-ttu-id="c5932-297">Nejprve při hello nástroj obsahuje výchozí hromadné importovat uložené procedury (BulkInsert.js), můžete se rozhodnout toospecify vlastní import uložené procedury:</span><span class="sxs-lookup"><span data-stu-id="c5932-297">First, while hello tool includes a default bulk import stored procedure (BulkInsert.js), you may choose toospecify your own import stored procedure:</span></span>

 ![Snímek obrazovky databázi Cosmos Azure hromadné vložení sproc možnost](./media/import-data/bulkinsertsp.png)

<span data-ttu-id="c5932-299">Kromě toho při importu datum typy (například ze systému SQL Server nebo MongoDB), můžete zvolit tři možnosti importu:</span><span class="sxs-lookup"><span data-stu-id="c5932-299">Additionally, when importing date types (e.g. from SQL Server or MongoDB), you can choose between three import options:</span></span>

 ![Snímek obrazovky databázi Cosmos Azure datum čas import možnosti](./media/import-data/datetimeoptions.png)

* <span data-ttu-id="c5932-301">Řetězec: Zachovat jako hodnotu řetězce</span><span class="sxs-lookup"><span data-stu-id="c5932-301">String: Persist as a string value</span></span>
* <span data-ttu-id="c5932-302">Epoch: Zachovat jako číslo hodnota Epoch</span><span class="sxs-lookup"><span data-stu-id="c5932-302">Epoch: Persist as an Epoch number value</span></span>
* <span data-ttu-id="c5932-303">Oba: Zachovat řetězec a Epoch číselné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="c5932-303">Both: Persist both string and Epoch number values.</span></span> <span data-ttu-id="c5932-304">Tato možnost vytvořit vnořený dokument, třeba: "date_joined": {"Value": "2013-10-21T21:17:25.2410000Z", "Epoch": 1382390245}</span><span class="sxs-lookup"><span data-stu-id="c5932-304">This option will create a subdocument, for example: "date_joined": { "Value": "2013-10-21T21:17:25.2410000Z", "Epoch": 1382390245 }</span></span>

<span data-ttu-id="c5932-305">Program pro import Azure Cosmos DB hromadné Hello má hello následující další rozšířené možnosti:</span><span class="sxs-lookup"><span data-stu-id="c5932-305">hello Azure Cosmos DB Bulk importer has hello following additional advanced options:</span></span>

1. <span data-ttu-id="c5932-306">Velikost dávky: hello nástroj výchozí tooa velikost dávky 50.</span><span class="sxs-lookup"><span data-stu-id="c5932-306">Batch Size: hello tool defaults tooa batch size of 50.</span></span>  <span data-ttu-id="c5932-307">Pokud toobe dokumenty hello importovat velké, zvažte, což snižuje velikost dávky hello.</span><span class="sxs-lookup"><span data-stu-id="c5932-307">If hello documents toobe imported are large, consider lowering hello batch size.</span></span> <span data-ttu-id="c5932-308">Pokud toobe dokumenty hello importovat malé, zvažte naopak vyvolání hello velikost dávky.</span><span class="sxs-lookup"><span data-stu-id="c5932-308">Conversely, if hello documents toobe imported are small, consider raising hello batch size.</span></span>
2. <span data-ttu-id="c5932-309">Skript maximální velikost (bajty): nástroj hello výchozí velikost maximální skriptu tooa 512KB</span><span class="sxs-lookup"><span data-stu-id="c5932-309">Max Script Size (bytes): hello tool defaults tooa max script size of 512KB</span></span>
3. <span data-ttu-id="c5932-310">Zakázat automatické generování Id: Pokud každý dokument toobe importovat obsahuje id pole, pak výběrem této možnosti můžete zvýšit výkon.</span><span class="sxs-lookup"><span data-stu-id="c5932-310">Disable Automatic Id Generation: If every document toobe imported contains an id field, then selecting this option can increase performance.</span></span> <span data-ttu-id="c5932-311">Chybí hodnota v poli jedinečné id dokumenty nebudou importovány.</span><span class="sxs-lookup"><span data-stu-id="c5932-311">Documents missing a unique id field will not be imported.</span></span>
4. <span data-ttu-id="c5932-312">Aktualizace stávající dokumenty: toonot výchozí nástroj hello nahrazení stávající dokumenty docházet ke konfliktům id.</span><span class="sxs-lookup"><span data-stu-id="c5932-312">Update Existing Documents: hello tool defaults toonot replacing existing documents with id conflicts.</span></span> <span data-ttu-id="c5932-313">Výběrem této možnosti umožníte přepsal stávající dokumenty s odpovídajícím ID.</span><span class="sxs-lookup"><span data-stu-id="c5932-313">Selecting this option will allow overwriting existing documents with matching ids.</span></span> <span data-ttu-id="c5932-314">Tato funkce je užitečná pro naplánované data migrace, které aktualizovat existující dokumenty.</span><span class="sxs-lookup"><span data-stu-id="c5932-314">This feature is useful for scheduled data migrations that update existing documents.</span></span>
5. <span data-ttu-id="c5932-315">Počet opakovaných pokusů o selhání: Určuje hello počet tooretry hello připojení tooAzure Cosmos DB v případě přechodných chyb (například přerušení připojení k síti).</span><span class="sxs-lookup"><span data-stu-id="c5932-315">Number of Retries on Failure: Specifies hello number of times tooretry hello connection tooAzure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
6. <span data-ttu-id="c5932-316">Interval opakování: Určuje, jak dlouho toowait mezi opakování hello připojení tooAzure Cosmos DB v případě přechodných chyb (například přerušení připojení k síti).</span><span class="sxs-lookup"><span data-stu-id="c5932-316">Retry Interval: Specifies how long toowait between retrying hello connection tooAzure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
7. <span data-ttu-id="c5932-317">Režim připojení: Určuje hello připojení režimu toouse s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="c5932-317">Connection Mode: Specifies hello connection mode toouse with Azure Cosmos DB.</span></span> <span data-ttu-id="c5932-318">jsou k dispozici možnosti Hello DirectTcp, DirectHttps a brány.</span><span class="sxs-lookup"><span data-stu-id="c5932-318">hello available choices are DirectTcp, DirectHttps, and Gateway.</span></span> <span data-ttu-id="c5932-319">režimy Hello přímé připojení jsou rychlejší, zatímco hello brány režim je další brány firewall popisný jako pouze používá port 443.</span><span class="sxs-lookup"><span data-stu-id="c5932-319">hello direct connection modes are faster, while hello gateway mode is more firewall friendly as it only uses port 443.</span></span>

![Snímek obrazovky databázi Cosmos Azure hromadným importem rozšířené možnosti](./media/import-data/docdbbulkoptions.png)

> [!TIP]
> <span data-ttu-id="c5932-321">Nástroj výchozí tooconnection režim DirectTcp import Hello.</span><span class="sxs-lookup"><span data-stu-id="c5932-321">hello import tool defaults tooconnection mode DirectTcp.</span></span> <span data-ttu-id="c5932-322">Pokud máte brány firewall problémy, přepněte tooconnection režimu bránu, vyžaduje jenom port 443.</span><span class="sxs-lookup"><span data-stu-id="c5932-322">If you experience firewall issues, switch tooconnection mode Gateway, as it only requires port 443.</span></span>
> 
> 

## <span data-ttu-id="c5932-323"><a id="DocumentDBSeqTarget"></a>tooimport toohello DocumentDB rozhraní API (po sobě jdoucích Import záznamů)</span><span class="sxs-lookup"><span data-stu-id="c5932-323"><a id="DocumentDBSeqTarget"></a>tooimport toohello DocumentDB API (Sequential Record Import)</span></span>
<span data-ttu-id="c5932-324">Program pro import po sobě jdoucích záznam Hello Azure Cosmos DB vám umožní tooimport z jakéhokoli z dostupných možností hello na základě záznamu podle.</span><span class="sxs-lookup"><span data-stu-id="c5932-324">hello Azure Cosmos DB sequential record importer allows you tooimport from any of hello available source options on a record by record basis.</span></span> <span data-ttu-id="c5932-325">Tuto možnost můžete zvolit, pokud importujete tooan existující kolekci, která dosáhla své kvóty uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="c5932-325">You might choose this option if you’re importing tooan existing collection that has reached its quota of stored procedures.</span></span> <span data-ttu-id="c5932-326">Hello nástroj podporuje import tooa jedna kolekce Azure Cosmos DB (jedním oddílem i více oddílu), stejně jako horizontálně dělené importu, které jsou data rozdělena mezi více kolekcí Azure Cosmos DB jedním oddílem nebo více oddílů.</span><span class="sxs-lookup"><span data-stu-id="c5932-326">hello tool supports import tooa single (both single-partition and multi-partition) Azure Cosmos DB collection, as well as sharded import whereby data is partitioned across multiple single-partition and/or multi-partition Azure Cosmos DB collections.</span></span> <span data-ttu-id="c5932-327">Další informace o segmentace dat naleznete v tématu [dělení a škálování v Azure Cosmos DB](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="c5932-327">For more information about partitioning data, see [Partitioning and scaling in Azure Cosmos DB](partition-data.md).</span></span>

![Snímek obrazovky databázi Cosmos Azure možnosti sekvenční záznamů importu](./media/import-data/documentdbsequential.png)

<span data-ttu-id="c5932-329">Formát Hello hello Azure Cosmos DB připojovacího řetězce je:</span><span class="sxs-lookup"><span data-stu-id="c5932-329">hello format of hello Azure Cosmos DB connection string is:</span></span>

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

<span data-ttu-id="c5932-330">Hello připojovací řetězce k účtu Azure Cosmos DB lze načíst z okna klíče hello hello portál Azure, jak je popsáno v [jak toomanage účet Azure Cosmos DB](manage-account.md), ale název hello hello databáze musí toohello toobe připojeno připojovací řetězec v hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="c5932-330">hello Azure Cosmos DB account connection string can be retrieved from hello Keys blade of hello Azure portal, as described in [How toomanage an Azure Cosmos DB account](manage-account.md), however hello name of hello database needs toobe appended toohello connection string in hello following format:</span></span>

    Database=<Azure Cosmos DB Database>;

> [!NOTE]
> <span data-ttu-id="c5932-331">Použití hello ověřte, zda příkaz tooensure, který hello zadané v poli řetězec připojení hello instanci Azure Cosmos databáze je přístupná.</span><span class="sxs-lookup"><span data-stu-id="c5932-331">Use hello Verify command tooensure that hello Azure Cosmos DB instance specified in hello connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="c5932-332">tooimport tooa jedna kolekce, zadejte název hello hello kolekce toowhich data budou naimportovány a klikněte na tlačítko Přidat hello.</span><span class="sxs-lookup"><span data-stu-id="c5932-332">tooimport tooa single collection, enter hello name of hello collection toowhich data will be imported and click hello Add button.</span></span> <span data-ttu-id="c5932-333">tooimport toomultiple kolekcí, zadejte jednotlivé názvy kolekce jednotlivě nebo použijte následující syntaxi toospecify hello více kolekcí: *collection_prefix*[index - end index počátečního].</span><span class="sxs-lookup"><span data-stu-id="c5932-333">tooimport toomultiple collections, either enter each collection name individually or use hello following syntax toospecify multiple collections: *collection_prefix*[start index - end index].</span></span> <span data-ttu-id="c5932-334">Při zadávání více kolekcí prostřednictvím hello výše uvedené syntaxe, mějte hello následující skutečnosti:</span><span class="sxs-lookup"><span data-stu-id="c5932-334">When specifying multiple collections via hello aforementioned syntax, keep hello following in mind:</span></span>

1. <span data-ttu-id="c5932-335">Jsou podporovány pouze celé číslo vzory názvů rozsahu.</span><span class="sxs-lookup"><span data-stu-id="c5932-335">Only integer range name patterns are supported.</span></span> <span data-ttu-id="c5932-336">Například zadání kolekce [0-3] vytvoří hello následující kolekce: collection0, collection1, collection2, collection3.</span><span class="sxs-lookup"><span data-stu-id="c5932-336">For example, specifying collection[0-3] will produce hello following collections: collection0, collection1, collection2, collection3.</span></span>
2. <span data-ttu-id="c5932-337">Můžete použít zkrácené syntaxe: kolekce [3] se emitování stejnou sadu kolekcí uveden v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="c5932-337">You can use an abbreviated syntax: collection[3] will emit same set of collections mentioned in step 1.</span></span>
3. <span data-ttu-id="c5932-338">Lze zadat více než jeden nahrazení.</span><span class="sxs-lookup"><span data-stu-id="c5932-338">More than one substitution can be provided.</span></span> <span data-ttu-id="c5932-339">Například kolekce [0-1] [0-9] vygeneruje 20 názvů kolekcí úvodními nulami (collection01,... 02... 03).</span><span class="sxs-lookup"><span data-stu-id="c5932-339">For example, collection[0-1] [0-9] will generate 20 collection names with leading zeros (collection01, ..02, ..03).</span></span>

<span data-ttu-id="c5932-340">Jakmile hello kolekce názvy byly nastaveny, vyberte požadované propustnost hello hello kolekcí (400 RUs too250, 000 RUs).</span><span class="sxs-lookup"><span data-stu-id="c5932-340">Once hello collection name(s) have been specified, choose hello desired throughput of hello collection(s) (400 RUs too250,000 RUs).</span></span> <span data-ttu-id="c5932-341">Pro nejlepší výkon import zvolte vyšší propustnost.</span><span class="sxs-lookup"><span data-stu-id="c5932-341">For best import performance, choose a higher throughput.</span></span> <span data-ttu-id="c5932-342">Další informace o úrovně výkonu najdete v tématu [úrovně výkonu v Azure Cosmos DB](performance-levels.md).</span><span class="sxs-lookup"><span data-stu-id="c5932-342">For more information about performance levels, see [Performance levels in Azure Cosmos DB](performance-levels.md).</span></span> <span data-ttu-id="c5932-343">Žádné importovat toocollections s propustností > 10 000 RUs bude vyžadovat klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="c5932-343">Any import toocollections with throughput >10,000 RUs will require a partition key.</span></span> <span data-ttu-id="c5932-344">Pokud vyberete více než 250 000 RUs toohave, budete potřebovat toofile žádost v portálu toohave hello vyšší váš účet.</span><span class="sxs-lookup"><span data-stu-id="c5932-344">If you choose toohave more than 250,000 RUs, you will need toofile a request in hello portal toohave your account increased.</span></span>

> [!NOTE]
> <span data-ttu-id="c5932-345">Hello propustnost nastavení se vztahuje pouze toocollection vytvoření.</span><span class="sxs-lookup"><span data-stu-id="c5932-345">hello throughput setting only applies toocollection creation.</span></span> <span data-ttu-id="c5932-346">Pokud hello zadané kolekce už existuje, se nezmění jeho propustnost.</span><span class="sxs-lookup"><span data-stu-id="c5932-346">If hello specified collection already exists, its throughput will not be modified.</span></span>
> 
> 

<span data-ttu-id="c5932-347">Při importu toomultiple kolekce, nástroj pro import hello podporuje hodnoty hash na základě horizontálního dělení.</span><span class="sxs-lookup"><span data-stu-id="c5932-347">When importing toomultiple collections, hello import tool supports hash based sharding.</span></span> <span data-ttu-id="c5932-348">V tomto scénáři, určete vlastnost dokumentu hello chcete toouse jako klíč oddílu hello (Pokud klíč oddílu je prázdné, dokumenty bude horizontálně dělené náhodně napříč hello cílové kolekce).</span><span class="sxs-lookup"><span data-stu-id="c5932-348">In this scenario, specify hello document property you wish toouse as hello Partition Key (if Partition Key is left blank, documents will be sharded randomly across hello target collections).</span></span>

<span data-ttu-id="c5932-349">Můžete volitelně můžete určit, které pole ve zdroji import hello má být použit jako hello vlastnost id dokumentu Azure Cosmos DB během importu hello (Všimněte si, že pokud dokumenty neobsahují tuto vlastnost, pak nástroj pro import hello bude generovat identifikátor GUID jako hodnota vlastnosti id hello).</span><span class="sxs-lookup"><span data-stu-id="c5932-349">You may optionally specify which field in hello import source should be used as hello Azure Cosmos DB document id property during hello import (note that if documents do not contain this property, then hello import tool will generate a GUID as hello id property value).</span></span>

<span data-ttu-id="c5932-350">Během importu nejsou k dispozici několik upřesňujících možností.</span><span class="sxs-lookup"><span data-stu-id="c5932-350">There are a number of advanced options available during import.</span></span> <span data-ttu-id="c5932-351">První při importu datum typy (například ze systému SQL Server nebo MongoDB), můžete zvolit tři možnosti importu:</span><span class="sxs-lookup"><span data-stu-id="c5932-351">First, when importing date types (e.g. from SQL Server or MongoDB), you can choose between three import options:</span></span>

 ![Snímek obrazovky databázi Cosmos Azure datum čas import možnosti](./media/import-data/datetimeoptions.png)

* <span data-ttu-id="c5932-353">Řetězec: Zachovat jako hodnotu řetězce</span><span class="sxs-lookup"><span data-stu-id="c5932-353">String: Persist as a string value</span></span>
* <span data-ttu-id="c5932-354">Epoch: Zachovat jako číslo hodnota Epoch</span><span class="sxs-lookup"><span data-stu-id="c5932-354">Epoch: Persist as an Epoch number value</span></span>
* <span data-ttu-id="c5932-355">Oba: Zachovat řetězec a Epoch číselné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="c5932-355">Both: Persist both string and Epoch number values.</span></span> <span data-ttu-id="c5932-356">Tato možnost vytvořit vnořený dokument, třeba: "date_joined": {"Value": "2013-10-21T21:17:25.2410000Z", "Epoch": 1382390245}</span><span class="sxs-lookup"><span data-stu-id="c5932-356">This option will create a subdocument, for example: "date_joined": { "Value": "2013-10-21T21:17:25.2410000Z", "Epoch": 1382390245 }</span></span>

<span data-ttu-id="c5932-357">Hello Azure Cosmos DB - program pro import po sobě jdoucích záznam má hello následující další rozšířené možnosti:</span><span class="sxs-lookup"><span data-stu-id="c5932-357">hello Azure Cosmos DB - Sequential record importer has hello following additional advanced options:</span></span>

1. <span data-ttu-id="c5932-358">Počet požadavků na paralelní: nástroj hello výchozí too2 paralelní požadavky.</span><span class="sxs-lookup"><span data-stu-id="c5932-358">Number of Parallel Requests: hello tool defaults too2 parallel requests.</span></span> <span data-ttu-id="c5932-359">Pokud toobe dokumenty hello importovat malé, zvažte vyvolání hello počet paralelní požadavků.</span><span class="sxs-lookup"><span data-stu-id="c5932-359">If hello documents toobe imported are small, consider raising hello number of parallel requests.</span></span> <span data-ttu-id="c5932-360">Všimněte si, že pokud toto číslo se vyvolá příliš mnoho hello importu může dojít k omezení šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="c5932-360">Note that if this number is raised too much, hello import may experience throttling.</span></span>
2. <span data-ttu-id="c5932-361">Zakázat automatické generování Id: Pokud každý dokument toobe importovat obsahuje id pole, pak výběrem této možnosti můžete zvýšit výkon.</span><span class="sxs-lookup"><span data-stu-id="c5932-361">Disable Automatic Id Generation: If every document toobe imported contains an id field, then selecting this option can increase performance.</span></span> <span data-ttu-id="c5932-362">Chybí hodnota v poli jedinečné id dokumenty nebudou importovány.</span><span class="sxs-lookup"><span data-stu-id="c5932-362">Documents missing a unique id field will not be imported.</span></span>
3. <span data-ttu-id="c5932-363">Aktualizace stávající dokumenty: toonot výchozí nástroj hello nahrazení stávající dokumenty docházet ke konfliktům id.</span><span class="sxs-lookup"><span data-stu-id="c5932-363">Update Existing Documents: hello tool defaults toonot replacing existing documents with id conflicts.</span></span> <span data-ttu-id="c5932-364">Výběrem této možnosti umožníte přepsal stávající dokumenty s odpovídajícím ID.</span><span class="sxs-lookup"><span data-stu-id="c5932-364">Selecting this option will allow overwriting existing documents with matching ids.</span></span> <span data-ttu-id="c5932-365">Tato funkce je užitečná pro naplánované data migrace, které aktualizovat existující dokumenty.</span><span class="sxs-lookup"><span data-stu-id="c5932-365">This feature is useful for scheduled data migrations that update existing documents.</span></span>
4. <span data-ttu-id="c5932-366">Počet opakovaných pokusů o selhání: Určuje hello počet tooretry hello připojení tooAzure Cosmos DB v případě přechodných chyb (například přerušení připojení k síti).</span><span class="sxs-lookup"><span data-stu-id="c5932-366">Number of Retries on Failure: Specifies hello number of times tooretry hello connection tooAzure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
5. <span data-ttu-id="c5932-367">Interval opakování: Určuje, jak dlouho toowait mezi opakování hello připojení tooAzure Cosmos DB v případě přechodných chyb (například přerušení připojení k síti).</span><span class="sxs-lookup"><span data-stu-id="c5932-367">Retry Interval: Specifies how long toowait between retrying hello connection tooAzure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
6. <span data-ttu-id="c5932-368">Režim připojení: Určuje hello připojení režimu toouse s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="c5932-368">Connection Mode: Specifies hello connection mode toouse with Azure Cosmos DB.</span></span> <span data-ttu-id="c5932-369">jsou k dispozici možnosti Hello DirectTcp, DirectHttps a brány.</span><span class="sxs-lookup"><span data-stu-id="c5932-369">hello available choices are DirectTcp, DirectHttps, and Gateway.</span></span> <span data-ttu-id="c5932-370">režimy Hello přímé připojení jsou rychlejší, zatímco hello brány režim je další brány firewall popisný jako pouze používá port 443.</span><span class="sxs-lookup"><span data-stu-id="c5932-370">hello direct connection modes are faster, while hello gateway mode is more firewall friendly as it only uses port 443.</span></span>

![Snímek obrazovky databázi Cosmos Azure import sekvenční záznam rozšířené možnosti](./media/import-data/documentdbsequentialoptions.png)

> [!TIP]
> <span data-ttu-id="c5932-372">Nástroj výchozí tooconnection režim DirectTcp import Hello.</span><span class="sxs-lookup"><span data-stu-id="c5932-372">hello import tool defaults tooconnection mode DirectTcp.</span></span> <span data-ttu-id="c5932-373">Pokud máte brány firewall problémy, přepněte tooconnection režimu bránu, vyžaduje jenom port 443.</span><span class="sxs-lookup"><span data-stu-id="c5932-373">If you experience firewall issues, switch tooconnection mode Gateway, as it only requires port 443.</span></span>
> 
> 

## <span data-ttu-id="c5932-374"><a id="IndexingPolicy"></a>Při vytváření kolekcí Azure Cosmos DB zadat zásady indexování</span><span class="sxs-lookup"><span data-stu-id="c5932-374"><a id="IndexingPolicy"></a>Specify an indexing policy when creating Azure Cosmos DB collections</span></span>
<span data-ttu-id="c5932-375">Když migrace hello povolíte během importu kolekce toocreate nástroj, můžete zadat zásady indexování hello hello kolekcí.</span><span class="sxs-lookup"><span data-stu-id="c5932-375">When you allow hello migration tool toocreate collections during import, you can specify hello indexing policy of hello collections.</span></span> <span data-ttu-id="c5932-376">V hello rozšířené možnosti části hello Azure Cosmos DB hromadného importu a možnosti Azure DB Cosmos po sobě jdoucích záznam přejděte části toohello zásady indexování.</span><span class="sxs-lookup"><span data-stu-id="c5932-376">In hello advanced options section of hello Azure Cosmos DB Bulk import and Azure Cosmos DB Sequential record options, navigate toohello Indexing Policy section.</span></span>

![Snímek obrazovky Azure Cosmos DB indexování zásad rozšířené možnosti](./media/import-data/indexingpolicy1.png)

<span data-ttu-id="c5932-378">Pomocí hello možnost Upřesnit zásady indexování, můžete vybrat indexování soubor zásad, ručně zadat zásady indexování nebo vybrat ze sady výchozích šablon (kliknutím pravým tlačítkem v hello indexování textbox zásad).</span><span class="sxs-lookup"><span data-stu-id="c5932-378">Using hello Indexing Policy advanced option, you can select an indexing policy file, manually enter an indexing policy, or select from a set of default templates (by right clicking in hello indexing policy textbox).</span></span>

<span data-ttu-id="c5932-379">šablony zásad Hello, poskytuje nástroj hello jsou:</span><span class="sxs-lookup"><span data-stu-id="c5932-379">hello policy templates hello tool provides are:</span></span>

* <span data-ttu-id="c5932-380">Výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="c5932-380">Default.</span></span> <span data-ttu-id="c5932-381">Tato zásada je vhodné, když jste provádění dotazy na rovnost pro řetězce a pomocí klauzule ORDER BY, rozsah a dotazy na rovnost pro čísla.</span><span class="sxs-lookup"><span data-stu-id="c5932-381">This policy is best when you’re performing equality queries against strings and using ORDER BY, range, and equality queries for numbers.</span></span> <span data-ttu-id="c5932-382">Tato zásada má nižší režijní náklady úložiště index než rozsah.</span><span class="sxs-lookup"><span data-stu-id="c5932-382">This policy has a lower index storage overhead than Range.</span></span>
* <span data-ttu-id="c5932-383">Rozsah.</span><span class="sxs-lookup"><span data-stu-id="c5932-383">Range.</span></span> <span data-ttu-id="c5932-384">Tato zásada je nejvhodnější, že používáte dotazy ORDER BY, rozsah a rovnost na čísla i řetězce.</span><span class="sxs-lookup"><span data-stu-id="c5932-384">This policy is best you’re using ORDER BY, range and equality queries on both numbers and strings.</span></span> <span data-ttu-id="c5932-385">Tato zásada nemá vyšší index režijní náklady na úložiště než výchozí nebo hodnoty Hash.</span><span class="sxs-lookup"><span data-stu-id="c5932-385">This policy has a higher index storage overhead than Default or Hash.</span></span>

![Snímek obrazovky Azure Cosmos DB indexování zásad rozšířené možnosti](./media/import-data/indexingpolicy2.png)

> [!NOTE]
> <span data-ttu-id="c5932-387">Pokud nezadáte zásady indexování, bude použita hello výchozí zásady.</span><span class="sxs-lookup"><span data-stu-id="c5932-387">If you do not specify an indexing policy, then hello default policy will be applied.</span></span> <span data-ttu-id="c5932-388">Další informace o indexování zásad najdete v tématu [Azure DB Cosmos indexování zásady](indexing-policies.md).</span><span class="sxs-lookup"><span data-stu-id="c5932-388">For more information about indexing policies, see [Azure Cosmos DB indexing policies](indexing-policies.md).</span></span>
> 
> 

## <a name="export-toojson-file"></a><span data-ttu-id="c5932-389">TooJSON souboru exportu</span><span class="sxs-lookup"><span data-stu-id="c5932-389">Export tooJSON file</span></span>
<span data-ttu-id="c5932-390">Hello Azure Cosmos DB JSON exportu vám umožní tooexport žádné hello dostupných možností tooa JSON souboru, který obsahuje pole dokumentů JSON.</span><span class="sxs-lookup"><span data-stu-id="c5932-390">hello Azure Cosmos DB JSON exporter allows you tooexport any of hello available source options tooa JSON file that contains an array of JSON documents.</span></span> <span data-ttu-id="c5932-391">Nástroj Hello zpracuje hello exportu pro vás, nebo můžete zvolit tooview hello výsledné migrace příkaz a spusťte příkaz hello sami.</span><span class="sxs-lookup"><span data-stu-id="c5932-391">hello tool will handle hello export for you, or you can choose tooview hello resulting migration command and run hello command yourself.</span></span> <span data-ttu-id="c5932-392">Výsledný soubor JSON Hello mohou být uloženy místně nebo v Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="c5932-392">hello resulting JSON file may be stored locally or in Azure Blob storage.</span></span>

![Snímek obrazovky z Azure Cosmos DB JSON možností exportu místního souboru](./media/import-data/jsontarget.png)

![Možností exportu snímek z Azure Cosmos DB JSON Azure Blob storage](./media/import-data/jsontarget2.png)

<span data-ttu-id="c5932-395">Volitelně můžete tooprettify hello výsledná formát JSON, což zvýší velikost hello hello výsledné dokumentu, při vytváření hello obsah další číselné vyjádření.</span><span class="sxs-lookup"><span data-stu-id="c5932-395">You may optionally choose tooprettify hello resulting JSON, which will increase hello size of hello resulting document while making hello contents more human readable.</span></span>

    Standard JSON export
    [{"id":"Sample","Title":"About Paris","Language":{"Name":"English"},"Author":{"Name":"Don","Location":{"City":"Paris","Country":"France"}},"Content":"Don's document in Azure Cosmos DB is a valid JSON document as defined by hello JSON spec.","PageViews":10000,"Topics":[{"Title":"History of Paris"},{"Title":"Places toosee in Paris"}]}]

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
    "Content": "Don's document in Azure Cosmos DB is a valid JSON document as defined by hello JSON spec.",
    "PageViews": 10000,
    "Topics": [
      {
        "Title": "History of Paris"
      },
      {
        "Title": "Places toosee in Paris"
      }
    ]
    }]

## <a name="advanced-configuration"></a><span data-ttu-id="c5932-396">Pokročilá konfigurace</span><span class="sxs-lookup"><span data-stu-id="c5932-396">Advanced configuration</span></span>
<span data-ttu-id="c5932-397">Na obrazovce pokročilou konfiguraci hello zadejte umístění hello toowhich souboru protokolu hello chcete všechny chyby zapsána.</span><span class="sxs-lookup"><span data-stu-id="c5932-397">In hello Advanced configuration screen, specify hello location of hello log file toowhich you would like any errors written.</span></span> <span data-ttu-id="c5932-398">Hello následující pravidla platí toothis stránky:</span><span class="sxs-lookup"><span data-stu-id="c5932-398">hello following rules apply toothis page:</span></span>

1. <span data-ttu-id="c5932-399">Pokud název souboru není k dispozici, pak všechny chyby, bude vrácen na stránce výsledky hello.</span><span class="sxs-lookup"><span data-stu-id="c5932-399">If a file name is not provided, then all errors will be returned on hello Results page.</span></span>
2. <span data-ttu-id="c5932-400">Pokud je název souboru zadaný bez adresáře, pak hello soubor se vytvoří (nebo přepsat) v aktuálním adresáři prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="c5932-400">If a file name is provided without a directory, then hello file will be created (or overwritten) in hello current environment directory.</span></span>
3. <span data-ttu-id="c5932-401">Pokud vyberete existující soubor, a soubor hello budou přepsány, není žádná možnost připojit.</span><span class="sxs-lookup"><span data-stu-id="c5932-401">If you select an existing file, then hello file will be overwritten, there is no append option.</span></span>

<span data-ttu-id="c5932-402">Pak zvolte zda toolog všechny, je důležité, nebo žádná chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="c5932-402">Then, choose whether toolog all, critical, or no error messages.</span></span> <span data-ttu-id="c5932-403">Nakonec rozhodnete, jak často hello na obrazovce přenos zprávy bude aktualizováno o jejím průběhu.</span><span class="sxs-lookup"><span data-stu-id="c5932-403">Finally, decide how frequently hello on screen transfer message will be updated with its progress.</span></span>

    ![Screenshot of Advanced configuration screen](./media/import-data/AdvancedConfiguration.png)

## <a name="confirm-import-settings-and-view-command-line"></a><span data-ttu-id="c5932-404">Potvrďte nastavení pro import a zobrazení příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="c5932-404">Confirm import settings and view command line</span></span>
1. <span data-ttu-id="c5932-405">Po zadání informace o zdroji, informace o cílové a pokročilou konfiguraci, zkontrolujte souhrn hello migrace a volitelně, zobrazení nebo zkopírování hello výsledná příkaz migrace (kopírování příkaz hello je užitečné tooautomate import operations):</span><span class="sxs-lookup"><span data-stu-id="c5932-405">After specifying source information, target information, and advanced configuration, review hello migration summary and, optionally, view/copy hello resulting migration command (copying hello command is useful tooautomate import operations):</span></span>
   
    ![Snímek obrazovky souhrn](./media/import-data/summary.png)
   
    ![Snímek obrazovky souhrn](./media/import-data/summarycommand.png)
2. <span data-ttu-id="c5932-408">Jakmile budete spokojeni s možnosti zdroje a cíle, klikněte na možnost **Import**.</span><span class="sxs-lookup"><span data-stu-id="c5932-408">Once you’re satisfied with your source and target options, click **Import**.</span></span> <span data-ttu-id="c5932-409">Hello uplynulý čas, přenášená počtu a informace o selhání (Pokud název souboru v pokročilé konfiguraci hello neposkytli) bude aktualizovat jako hello importu je v procesu.</span><span class="sxs-lookup"><span data-stu-id="c5932-409">hello elapsed time, transferred count, and failure information (if you didn't provide a file name in hello Advanced configuration) will update as hello import is in process.</span></span> <span data-ttu-id="c5932-410">Po dokončení můžete exportovat výsledky hello (např. toodeal s jeho selhání import).</span><span class="sxs-lookup"><span data-stu-id="c5932-410">Once complete, you can export hello results (e.g. toodeal with any import failures).</span></span>
   
    ![Snímek obrazovky z Azure Cosmos DB JSON možností exportu](./media/import-data/viewresults.png)
3. <span data-ttu-id="c5932-412">Nový import, může spustit také udržuje hello stávající nastavení (například připojovací řetězec informace, zdroje a cíle volba atd.) nebo resetování všechny hodnoty.</span><span class="sxs-lookup"><span data-stu-id="c5932-412">You may also start a new import, either keeping hello existing settings (e.g. connection string information, source and target choice, etc.) or resetting all values.</span></span>
   
    ![Snímek obrazovky z Azure Cosmos DB JSON možností exportu](./media/import-data/newimport.png)

## <a name="next-steps"></a><span data-ttu-id="c5932-414">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c5932-414">Next steps</span></span>

<span data-ttu-id="c5932-415">V tomto kurzu provedete krok hello následující:</span><span class="sxs-lookup"><span data-stu-id="c5932-415">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c5932-416">Nainstalovaný nástroj pro migraci dat hello</span><span class="sxs-lookup"><span data-stu-id="c5932-416">Installed hello Data Migration tool</span></span>
> * <span data-ttu-id="c5932-417">Importovaná data z různých zdrojů dat.</span><span class="sxs-lookup"><span data-stu-id="c5932-417">Imported data from different data sources</span></span>
> * <span data-ttu-id="c5932-418">Export z Azure Cosmos DB tooJSON</span><span class="sxs-lookup"><span data-stu-id="c5932-418">Exported from Azure Cosmos DB tooJSON</span></span>

<span data-ttu-id="c5932-419">Teď můžete pokračovat dalším kurzu toohello a zjistěte, jak tooquery dat pomocí Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="c5932-419">You can now proceed toohello next tutorial and learn how tooquery data using Azure Cosmos DB.</span></span> 

> [!div class="nextstepaction"]
>[<span data-ttu-id="c5932-420">Jak tooquery dat?</span><span class="sxs-lookup"><span data-stu-id="c5932-420">How tooquery data?</span></span>](../cosmos-db/tutorial-query-documentdb.md)
