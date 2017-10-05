---
title: "Kurzu NoSQL: DocumentDB rozhraní API pro Azure Cosmos DB Java SDK | Microsoft Docs"
description: "Kurz NoSQL, která vytváří online databáze a Konzolová aplikace Java pomocí DocumentDB rozhraní API pro Azure Cosmos DB. Azure DocumentDB je databáze NoSQL pro JSON."
keywords: nosql tutorial, online database, java console application
services: cosmos-db
documentationcenter: Java
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: 75a9efa1-7edd-4fed-9882-c0177274cbb2
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 05/22/2017
ms.author: arramac
ms.openlocfilehash: 5c4bcda308f001572e1c34e991616fc209250a02
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="nosql-tutorial-build-a-documentdb-api-java-console-application"></a><span data-ttu-id="9b5be-105">Kurzu NoSQL: vytvoření konzolové aplikace DocumentDB Java rozhraní API</span><span class="sxs-lookup"><span data-stu-id="9b5be-105">NoSQL tutorial: Build a DocumentDB API Java console application</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="9b5be-106">.NET</span><span class="sxs-lookup"><span data-stu-id="9b5be-106">.NET</span></span>](documentdb-get-started.md)
> * [<span data-ttu-id="9b5be-107">.NET Core</span><span class="sxs-lookup"><span data-stu-id="9b5be-107">.NET Core</span></span>](documentdb-dotnetcore-get-started.md)
> * [<span data-ttu-id="9b5be-108">Node.js pro MongoDB</span><span class="sxs-lookup"><span data-stu-id="9b5be-108">Node.js for MongoDB</span></span>](mongodb-samples.md)
> * [<span data-ttu-id="9b5be-109">Node.js</span><span class="sxs-lookup"><span data-stu-id="9b5be-109">Node.js</span></span>](documentdb-nodejs-get-started.md)
> * [<span data-ttu-id="9b5be-110">Java</span><span class="sxs-lookup"><span data-stu-id="9b5be-110">Java</span></span>](documentdb-java-get-started.md)
> * [<span data-ttu-id="9b5be-111">C++</span><span class="sxs-lookup"><span data-stu-id="9b5be-111">C++</span></span>](documentdb-cpp-get-started.md)
>  
> 

<span data-ttu-id="9b5be-112">Vítejte v kurzu NoSQL pro DocumentDB rozhraní API pro Azure Cosmos DB Java SDK!</span><span class="sxs-lookup"><span data-stu-id="9b5be-112">Welcome to the NoSQL tutorial for the DocumentDB API for Azure Cosmos DB Java SDK!</span></span> <span data-ttu-id="9b5be-113">Až projdete tímto kurzem, budete mít konzolovou aplikaci, která vytváří prostředky Azure Cosmos DB a dotazuje se na ně.</span><span class="sxs-lookup"><span data-stu-id="9b5be-113">After following this tutorial, you'll have a console application that creates and queries Azure Cosmos DB resources.</span></span>

<span data-ttu-id="9b5be-114">Kurz zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="9b5be-114">We cover:</span></span>

* <span data-ttu-id="9b5be-115">Vytvoření účtu služby Azure Cosmos DB a připojení k němu</span><span class="sxs-lookup"><span data-stu-id="9b5be-115">Creating and connecting to an Azure Cosmos DB account</span></span>
* <span data-ttu-id="9b5be-116">Konfigurace řešení v nástroji Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9b5be-116">Configuring your Visual Studio Solution</span></span>
* <span data-ttu-id="9b5be-117">Vytvoření online databáze</span><span class="sxs-lookup"><span data-stu-id="9b5be-117">Creating an online database</span></span>
* <span data-ttu-id="9b5be-118">Vytvoření kolekce</span><span class="sxs-lookup"><span data-stu-id="9b5be-118">Creating a collection</span></span>
* <span data-ttu-id="9b5be-119">Vytvoření dokumentů JSON</span><span class="sxs-lookup"><span data-stu-id="9b5be-119">Creating JSON documents</span></span>
* <span data-ttu-id="9b5be-120">Dotazování na kolekci</span><span class="sxs-lookup"><span data-stu-id="9b5be-120">Querying the collection</span></span>
* <span data-ttu-id="9b5be-121">Vytvoření dokumentů JSON</span><span class="sxs-lookup"><span data-stu-id="9b5be-121">Creating JSON documents</span></span>
* <span data-ttu-id="9b5be-122">Dotazování na kolekci</span><span class="sxs-lookup"><span data-stu-id="9b5be-122">Querying the collection</span></span>
* <span data-ttu-id="9b5be-123">Nahrazení dokumentu</span><span class="sxs-lookup"><span data-stu-id="9b5be-123">Replacing a document</span></span>
* <span data-ttu-id="9b5be-124">Odstranění dokumentu</span><span class="sxs-lookup"><span data-stu-id="9b5be-124">Deleting a document</span></span>
* <span data-ttu-id="9b5be-125">Odstranění databáze</span><span class="sxs-lookup"><span data-stu-id="9b5be-125">Deleting the database</span></span>

<span data-ttu-id="9b5be-126">Můžeme začít!</span><span class="sxs-lookup"><span data-stu-id="9b5be-126">Now let's get started!</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9b5be-127">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9b5be-127">Prerequisites</span></span>
<span data-ttu-id="9b5be-128">Ujistěte se, že máte následující:</span><span class="sxs-lookup"><span data-stu-id="9b5be-128">Make sure you have the following:</span></span>

* <span data-ttu-id="9b5be-129">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="9b5be-129">An active Azure account.</span></span> <span data-ttu-id="9b5be-130">Pokud žádný nemáte, můžete si zaregistrovat [bezplatný účet](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="9b5be-130">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span> <span data-ttu-id="9b5be-131">Alternativně můžete pro tento kurz použít [emulátor služby Azure Cosmos DB](local-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="9b5be-131">Alternatively, you can use the [Azure Cosmos DB Emulator](local-emulator.md) for this tutorial.</span></span>
* [<span data-ttu-id="9b5be-132">Git</span><span class="sxs-lookup"><span data-stu-id="9b5be-132">Git</span></span>](https://git-scm.com/downloads)
* <span data-ttu-id="9b5be-133">[Java Development Kit (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html)</span><span class="sxs-lookup"><span data-stu-id="9b5be-133">[Java Development Kit (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span></span>
* <span data-ttu-id="9b5be-134">[Maven](http://maven.apache.org/download.cgi).</span><span class="sxs-lookup"><span data-stu-id="9b5be-134">[Maven](http://maven.apache.org/download.cgi).</span></span>

## <a name="step-1-create-an-azure-cosmos-db-account"></a><span data-ttu-id="9b5be-135">Krok 1: Vytvoření účtu služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="9b5be-135">Step 1: Create an Azure Cosmos DB account</span></span>
<span data-ttu-id="9b5be-136">Vytvořme účet služby Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="9b5be-136">Let's create an Azure Cosmos DB account.</span></span> <span data-ttu-id="9b5be-137">Pokud už máte účet, který chcete použít, můžete přeskočit k části [Klonování projektu z GitHubu](#GitClone).</span><span class="sxs-lookup"><span data-stu-id="9b5be-137">If you already have an account you want to use, you can skip ahead to [Clone the GitHub project](#GitClone).</span></span> <span data-ttu-id="9b5be-138">Pokud používáte emulátor služby Azure Cosmos DB, nastavte emulátor pomocí postupu v tématu [Emulátor služby Azure Cosmos DB](local-emulator.md) a přeskočte k části [Klonování projektu z GitHubu](#GitClone).</span><span class="sxs-lookup"><span data-stu-id="9b5be-138">If you are using the Azure Cosmos DB Emulator, follow the steps at [Azure Cosmos DB Emulator](local-emulator.md) to set up the emulator and skip ahead to [Clone the GitHub project](#GitClone).</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <span data-ttu-id="9b5be-139"><a id="GitClone"></a>Krok 2: Klonování projektu z GitHubu</span><span class="sxs-lookup"><span data-stu-id="9b5be-139"><a id="GitClone"></a>Step 2: Clone the GitHub project</span></span>
<span data-ttu-id="9b5be-140">Můžete začít naklonováním úložiště GitHub pro projekt [Začínáme se službou Azure Cosmos DB a Javou](https://github.com/Azure-Samples/documentdb-java-getting-started).</span><span class="sxs-lookup"><span data-stu-id="9b5be-140">You can get started by cloning the GitHub repository for [Get Started with Azure Cosmos DB and Java](https://github.com/Azure-Samples/documentdb-java-getting-started).</span></span> <span data-ttu-id="9b5be-141">Například spusťte z místního adresáře následující příkaz, který načte ukázkový projekt pro místní použití.</span><span class="sxs-lookup"><span data-stu-id="9b5be-141">For example, from a local directory run the following to retrieve the sample project locally.</span></span>

    git clone git@github.com:Azure-Samples/azure-cosmos-db-documentdb-java-getting-started.git

    cd azure-cosmos-db-documentdb-java-getting-started

<span data-ttu-id="9b5be-142">Obsahuje adresáři `pom.xml` pro projekt a `src` složku obsahující Java zdrojového kódu včetně `Program.java` které ukazuje, jak provádějí jednoduché operace s Azure DB Cosmos jako je vytváření dokumentů a dotazování na data v rámci kolekce .</span><span class="sxs-lookup"><span data-stu-id="9b5be-142">The directory contains a `pom.xml` for the project and a `src` folder containing Java source code including `Program.java` which shows how perform simple operations with Azure Cosmos DB like creating documents and querying data within a collection.</span></span> <span data-ttu-id="9b5be-143">Soubor `pom.xml` obsahuje závislost na sadu [DocumentDB Java SDK v nástroji Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb).</span><span class="sxs-lookup"><span data-stu-id="9b5be-143">The `pom.xml` includes a dependency on the [DocumentDB Java SDK on Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb).</span></span>

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-documentdb</artifactId>
        <version>LATEST</version>
    </dependency>

## <span data-ttu-id="9b5be-144"><a id="Connect"></a>Krok 3: Připojení k účtu služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="9b5be-144"><a id="Connect"></a>Step 3: Connect to an Azure Cosmos DB account</span></span>
<span data-ttu-id="9b5be-145">Dále přejděte zpět na [Azure Portal](https://portal.azure.com) a získejte koncový bod a primární hlavní klíč.</span><span class="sxs-lookup"><span data-stu-id="9b5be-145">Next, head back to the [Azure Portal](https://portal.azure.com) to retrieve your endpoint and primary master key.</span></span> <span data-ttu-id="9b5be-146">Koncový bod a primární klíč služby Azure Cosmos DB jsou potřeba k tomu, aby aplikace věděla, kam se má připojit, a aby služba Azure Cosmos DB důvěřovala připojení aplikace.</span><span class="sxs-lookup"><span data-stu-id="9b5be-146">The Azure Cosmos DB endpoint and primary key are necessary for your application to understand where to connect to, and for Azure Cosmos DB to trust your application's connection.</span></span>

<span data-ttu-id="9b5be-147">Na webu Azure Portal přejděte do účtu služby Azure Cosmos DB a klikněte na **Klíče**.</span><span class="sxs-lookup"><span data-stu-id="9b5be-147">In the Azure Portal, navigate to your Azure Cosmos DB account, and then click **Keys**.</span></span> <span data-ttu-id="9b5be-148">Zkopírujte identifikátor URI z portálu a vložte ho do `https://FILLME.documents.azure.com` v souboru Program.java.</span><span class="sxs-lookup"><span data-stu-id="9b5be-148">Copy the URI from the portal and paste it into `https://FILLME.documents.azure.com` in the Program.java file.</span></span> <span data-ttu-id="9b5be-149">Poté zkopírujte PRIMÁRNÍ KLÍČ z portálu a vložte ho do `FILLME`.</span><span class="sxs-lookup"><span data-stu-id="9b5be-149">Then copy the PRIMARY KEY from the portal and paste it into `FILLME`.</span></span>

    this.client = new DocumentClient(
        "https://FILLME.documents.azure.com",
        "FILLME"
        , new ConnectionPolicy(),
        ConsistencyLevel.Session);

![Snímek obrazovky webu Azure Portal, který se v kurzu NoSQL používá k vytvoření konzolové aplikace v jazyce Java.][keys]

## <a name="step-4-create-a-database"></a><span data-ttu-id="9b5be-152">Krok 4: Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="9b5be-152">Step 4: Create a database</span></span>
<span data-ttu-id="9b5be-153">[Databázi](documentdb-resources.md#databases) Azure Cosmos DB je možné vytvořit pomocí metody [createDatabase](/java/api/com.microsoft.azure.documentdb._document_client.createdatabase) třídy **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="9b5be-153">Your Azure Cosmos DB [database](documentdb-resources.md#databases) can be created by using the [createDatabase](/java/api/com.microsoft.azure.documentdb._document_client.createdatabase) method of the **DocumentClient** class.</span></span> <span data-ttu-id="9b5be-154">Databáze je logický kontejner úložiště dokumentů JSON rozděleného mezi kolekcemi.</span><span class="sxs-lookup"><span data-stu-id="9b5be-154">A database is the logical container of JSON document storage partitioned across collections.</span></span>

    Database database = new Database();
    database.setId("familydb");
    this.client.createDatabase(database, null);

## <span data-ttu-id="9b5be-155"><a id="CreateColl"></a>Krok 5: Vytvoření kolekce</span><span class="sxs-lookup"><span data-stu-id="9b5be-155"><a id="CreateColl"></a>Step 5: Create a collection</span></span>
> [!WARNING]
> <span data-ttu-id="9b5be-156">**createCollection** vytvoří novou kolekci s vyhrazenou propustností, za kterou se hradí poplatky.</span><span class="sxs-lookup"><span data-stu-id="9b5be-156">**createCollection** creates a new collection with reserved throughput, which has pricing implications.</span></span> <span data-ttu-id="9b5be-157">Další podrobnosti najdete na [stránce s cenami](https://azure.microsoft.com/pricing/details/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="9b5be-157">For more details, visit our [pricing page](https://azure.microsoft.com/pricing/details/cosmos-db/).</span></span>
> 
> 

<span data-ttu-id="9b5be-158">[Kolekci](documentdb-resources.md#collections) je možné vytvořit pomocí metody [createCollection](/java/api/com.microsoft.azure.documentdb._document_client.createcollection) třídy **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="9b5be-158">A [collection](documentdb-resources.md#collections) can be created by using the [createCollection](/java/api/com.microsoft.azure.documentdb._document_client.createcollection) method of the **DocumentClient** class.</span></span> <span data-ttu-id="9b5be-159">Kolekce je kontejner dokumentů JSON a přidružené logiky javascriptové aplikace.</span><span class="sxs-lookup"><span data-stu-id="9b5be-159">A collection is a container of JSON documents and associated JavaScript application logic.</span></span>


    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId("familycoll");

    // Azure Cosmos DB collections can be reserved with throughput specified in request units/second. 
    // Here we create a collection with 400 RU/s.
    RequestOptions requestOptions = new RequestOptions();
    requestOptions.setOfferThroughput(400);

    this.client.createCollection("/dbs/familydb", collectionInfo, requestOptions);

## <span data-ttu-id="9b5be-160"><a id="CreateDoc"></a>Krok 6: Vytvoření dokumentů JSON</span><span class="sxs-lookup"><span data-stu-id="9b5be-160"><a id="CreateDoc"></a>Step 6: Create JSON documents</span></span>
<span data-ttu-id="9b5be-161">[Dokument](documentdb-resources.md#documents) je možné vytvořit pomocí metody [createDocument](/java/api/com.microsoft.azure.documentdb._document_client.createdocument) třídy **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="9b5be-161">A [document](documentdb-resources.md#documents) can be created by using the [createDocument](/java/api/com.microsoft.azure.documentdb._document_client.createdocument) method of the **DocumentClient** class.</span></span> <span data-ttu-id="9b5be-162">Dokumenty představují uživatelem definovaný (libovolný) obsah JSON.</span><span class="sxs-lookup"><span data-stu-id="9b5be-162">Documents are user-defined (arbitrary) JSON content.</span></span> <span data-ttu-id="9b5be-163">Nyní můžete vložit jeden nebo více dokumentů.</span><span class="sxs-lookup"><span data-stu-id="9b5be-163">We can now insert one or more documents.</span></span> <span data-ttu-id="9b5be-164">Pokud již máte data, která chcete uložit do databáze, můžete použít Azure Cosmos DB [nástroj pro migraci dat](import-data.md) pro import dat do databáze.</span><span class="sxs-lookup"><span data-stu-id="9b5be-164">If you already have data you'd like to store in your database, you can use Azure Cosmos DB's [Data Migration tool](import-data.md) to import the data into a database.</span></span>

    // Insert your Java objects as documents 
    Family andersenFamily = new Family();
    andersenFamily.setId("Andersen.1");
    andersenFamily.setLastName("Andersen")

    // More initialization skipped for brevity. You can have nested references
    andersenFamily.setParents(new Parent[] { parent1, parent2 });
    andersenFamily.setDistrict("WA5");
    Address address = new Address();
    address.setCity("Seattle");
    address.setCounty("King");
    address.setState("WA");

    andersenFamily.setAddress(address);
    andersenFamily.setRegistered(true);

    this.client.createDocument("/dbs/familydb/colls/familycoll", family, new RequestOptions(), true);

![Diagram ilustrující hierarchický vztah mezi účtem, online databází, kolekcí a dokumenty používanými v kurzu NoSQL k vytvoření konzolové aplikace v jazyce Java](./media/documentdb-get-started/nosql-tutorial-account-database.png)

## <span data-ttu-id="9b5be-166"><a id="Query"></a>Krok 7: Dotazování prostředků Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="9b5be-166"><a id="Query"></a>Step 7: Query Azure Cosmos DB resources</span></span>
<span data-ttu-id="9b5be-167">Azure Cosmos DB podporuje bohaté [dotazy](documentdb-sql-query.md) na dokumenty JSON uložené v každé z kolekcí.</span><span class="sxs-lookup"><span data-stu-id="9b5be-167">Azure Cosmos DB supports rich [queries](documentdb-sql-query.md) against JSON documents stored in each collection.</span></span>  <span data-ttu-id="9b5be-168">Následující ukázkový kód ukazuje dotazování na dokumenty ve službě Azure Cosmos DB pomocí syntaxe SQL a metody [queryDocuments](/java/api/com.microsoft.azure.documentdb._document_client.querydocuments).</span><span class="sxs-lookup"><span data-stu-id="9b5be-168">The following sample code shows how to query documents in Azure Cosmos DB using SQL syntax with the [queryDocuments](/java/api/com.microsoft.azure.documentdb._document_client.querydocuments) method.</span></span>

    FeedResponse<Document> queryResults = this.client.queryDocuments(
        "/dbs/familydb/colls/familycoll",
        "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", 
        null);

    System.out.println("Running SQL query...");
    for (Document family : queryResults.getQueryIterable()) {
        System.out.println(String.format("\tRead %s", family));
    }

## <span data-ttu-id="9b5be-169"><a id="ReplaceDocument"></a>Krok 8: Nahrazení dokumentu JSON</span><span class="sxs-lookup"><span data-stu-id="9b5be-169"><a id="ReplaceDocument"></a>Step 8: Replace JSON document</span></span>
<span data-ttu-id="9b5be-170">Azure Cosmos DB podporuje aktualizaci dokumentů JSON pomocí metody [replaceDocument](/java/api/com.microsoft.azure.documentdb._document_client.replacedocument).</span><span class="sxs-lookup"><span data-stu-id="9b5be-170">Azure Cosmos DB supports updating JSON documents using the [replaceDocument](/java/api/com.microsoft.azure.documentdb._document_client.replacedocument) method.</span></span>

    // Update a property
    andersenFamily.Children[0].Grade = 6;

    this.client.replaceDocument(
        "/dbs/familydb/colls/familycoll/docs/Andersen.1", 
        andersenFamily,
        null);

## <span data-ttu-id="9b5be-171"><a id="DeleteDocument"></a>Krok 9: Odstranění dokumentu JSON</span><span class="sxs-lookup"><span data-stu-id="9b5be-171"><a id="DeleteDocument"></a>Step 9: Delete JSON document</span></span>
<span data-ttu-id="9b5be-172">Podobně Azure Cosmos DB podporuje odstraňování dokumentů JSON pomocí metody [deleteDocument](/java/api/com.microsoft.azure.documentdb._document_client.deletedocument).</span><span class="sxs-lookup"><span data-stu-id="9b5be-172">Similarly, Azure Cosmos DB supports deleting JSON documents using the [deleteDocument](/java/api/com.microsoft.azure.documentdb._document_client.deletedocument) method.</span></span>  

    this.client.delete("/dbs/familydb/colls/familycoll/docs/Andersen.1", null);

## <span data-ttu-id="9b5be-173"><a id="DeleteDatabase"></a>Krok 10: Odstranění databáze</span><span class="sxs-lookup"><span data-stu-id="9b5be-173"><a id="DeleteDatabase"></a>Step 10: Delete the database</span></span>
<span data-ttu-id="9b5be-174">Odstraněním vytvořené databáze dojde k odebrání databáze a všech jejích podřízených prostředků (kolekcí, dokumentů atd.).</span><span class="sxs-lookup"><span data-stu-id="9b5be-174">Deleting the created database removes the database and all children resources (collections, documents, etc.).</span></span>

    this.client.deleteDatabase("/dbs/familydb", null);

## <span data-ttu-id="9b5be-175"><a id="Run"></a>Krok 11: Spuštění celé konzolové aplikace jazyka Java!</span><span class="sxs-lookup"><span data-stu-id="9b5be-175"><a id="Run"></a>Step 11: Run your Java console application all together!</span></span>
<span data-ttu-id="9b5be-176">Ke spuštění aplikace z konzoly, přejděte do složky projektu a zkompilovat pomocí nástroje Maven:</span><span class="sxs-lookup"><span data-stu-id="9b5be-176">To run the application from the console, navigate to the project folder and compile using Maven:</span></span>
    
    mvn package

<span data-ttu-id="9b5be-177">Spuštěním příkazu `mvn package` se z nástroje Maven stáhne nejnovější verze knihovny Azure Cosmos DB a vytvoří se soubor `GetStarted-0.0.1-SNAPSHOT.jar`.</span><span class="sxs-lookup"><span data-stu-id="9b5be-177">Running `mvn package` downloads the latest Azure Cosmos DB library from Maven and produces `GetStarted-0.0.1-SNAPSHOT.jar`.</span></span> <span data-ttu-id="9b5be-178">Potom spusťte aplikaci následujícím příkazem:</span><span class="sxs-lookup"><span data-stu-id="9b5be-178">Then run the app by running:</span></span>

    mvn exec:java -D exec.mainClass=GetStarted.Program

<span data-ttu-id="9b5be-179">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="9b5be-179">Congratulations!</span></span> <span data-ttu-id="9b5be-180">Dokončili jste tento kurz NoSQL a máte funkční konzolovou aplikaci jazyka Java!</span><span class="sxs-lookup"><span data-stu-id="9b5be-180">You've completed this NoSQL tutorial and have a working Java console application!</span></span>

## <a name="next-steps"></a><span data-ttu-id="9b5be-181">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9b5be-181">Next steps</span></span>
* <span data-ttu-id="9b5be-182">Chcete kurz vývoje webové aplikace v jazyce Java?</span><span class="sxs-lookup"><span data-stu-id="9b5be-182">Want a Java web app tutorial?</span></span> <span data-ttu-id="9b5be-183">Viz [Vytvoření webové aplikace pomocí Javy a služby Azure Cosmos DB](documentdb-java-application.md).</span><span class="sxs-lookup"><span data-stu-id="9b5be-183">See [Build a web application with Java using Azure Cosmos DB](documentdb-java-application.md).</span></span>
* <span data-ttu-id="9b5be-184">Zjistěte, jak [monitorovat účet služby Azure Cosmos DB](monitor-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="9b5be-184">Learn how to [monitor an Azure Cosmos DB account](monitor-accounts.md).</span></span>
* <span data-ttu-id="9b5be-185">Spouštějte dotazy proti ukázkovým datovým sadám v [Query Playground](https://www.documentdb.com/sql/demo).</span><span class="sxs-lookup"><span data-stu-id="9b5be-185">Run queries against our sample dataset in the [Query Playground](https://www.documentdb.com/sql/demo).</span></span>
* <span data-ttu-id="9b5be-186">Přečtěte si více o tomto programovacím modelu v části Vyvíjejte na [stránce dokumentace ke službě Azure Cosmos DB](https://azure.microsoft.com/documentation/services/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="9b5be-186">Learn more about the programming model in the Develop section of the [Azure Cosmos DB documentation page](https://azure.microsoft.com/documentation/services/documentdb/).</span></span>

[keys]: media/documentdb-get-started/nosql-tutorial-keys.png
