---
title: "Kurzu NoSQL: DocumentDB rozhraní API pro Azure Cosmos DB Java SDK | Microsoft Docs"
description: "Kurz NoSQL, která vytváří online databáze a Konzolová aplikace Java pomocí hello DocumentDB rozhraní API pro Azure Cosmos DB. Azure DocumentDB je databáze NoSQL pro JSON."
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
ms.openlocfilehash: 1a298a15ab911d140b9df30ad52cfe0fa07c55b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="nosql-tutorial-build-a-documentdb-api-java-console-application"></a><span data-ttu-id="1005c-105">Kurzu NoSQL: vytvoření konzolové aplikace DocumentDB Java rozhraní API</span><span class="sxs-lookup"><span data-stu-id="1005c-105">NoSQL tutorial: Build a DocumentDB API Java console application</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1005c-106">.NET</span><span class="sxs-lookup"><span data-stu-id="1005c-106">.NET</span></span>](documentdb-get-started.md)
> * [<span data-ttu-id="1005c-107">.NET Core</span><span class="sxs-lookup"><span data-stu-id="1005c-107">.NET Core</span></span>](documentdb-dotnetcore-get-started.md)
> * [<span data-ttu-id="1005c-108">Node.js pro MongoDB</span><span class="sxs-lookup"><span data-stu-id="1005c-108">Node.js for MongoDB</span></span>](mongodb-samples.md)
> * [<span data-ttu-id="1005c-109">Node.js</span><span class="sxs-lookup"><span data-stu-id="1005c-109">Node.js</span></span>](documentdb-nodejs-get-started.md)
> * [<span data-ttu-id="1005c-110">Java</span><span class="sxs-lookup"><span data-stu-id="1005c-110">Java</span></span>](documentdb-java-get-started.md)
> * [<span data-ttu-id="1005c-111">C++</span><span class="sxs-lookup"><span data-stu-id="1005c-111">C++</span></span>](documentdb-cpp-get-started.md)
>  
> 

<span data-ttu-id="1005c-112">Vítejte v kurzu NoSQL toohello pro hello DocumentDB rozhraní API pro Azure Cosmos DB Java SDK!</span><span class="sxs-lookup"><span data-stu-id="1005c-112">Welcome toohello NoSQL tutorial for hello DocumentDB API for Azure Cosmos DB Java SDK!</span></span> <span data-ttu-id="1005c-113">Až projdete tímto kurzem, budete mít konzolovou aplikaci, která vytváří prostředky Azure Cosmos DB a dotazuje se na ně.</span><span class="sxs-lookup"><span data-stu-id="1005c-113">After following this tutorial, you'll have a console application that creates and queries Azure Cosmos DB resources.</span></span>

<span data-ttu-id="1005c-114">Kurz zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="1005c-114">We cover:</span></span>

* <span data-ttu-id="1005c-115">Vytvoření a připojení účtu Azure Cosmos DB tooan</span><span class="sxs-lookup"><span data-stu-id="1005c-115">Creating and connecting tooan Azure Cosmos DB account</span></span>
* <span data-ttu-id="1005c-116">Konfigurace řešení v nástroji Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1005c-116">Configuring your Visual Studio Solution</span></span>
* <span data-ttu-id="1005c-117">Vytvoření online databáze</span><span class="sxs-lookup"><span data-stu-id="1005c-117">Creating an online database</span></span>
* <span data-ttu-id="1005c-118">Vytvoření kolekce</span><span class="sxs-lookup"><span data-stu-id="1005c-118">Creating a collection</span></span>
* <span data-ttu-id="1005c-119">Vytvoření dokumentů JSON</span><span class="sxs-lookup"><span data-stu-id="1005c-119">Creating JSON documents</span></span>
* <span data-ttu-id="1005c-120">Dotazování na kolekci hello</span><span class="sxs-lookup"><span data-stu-id="1005c-120">Querying hello collection</span></span>
* <span data-ttu-id="1005c-121">Vytvoření dokumentů JSON</span><span class="sxs-lookup"><span data-stu-id="1005c-121">Creating JSON documents</span></span>
* <span data-ttu-id="1005c-122">Dotazování na kolekci hello</span><span class="sxs-lookup"><span data-stu-id="1005c-122">Querying hello collection</span></span>
* <span data-ttu-id="1005c-123">Nahrazení dokumentu</span><span class="sxs-lookup"><span data-stu-id="1005c-123">Replacing a document</span></span>
* <span data-ttu-id="1005c-124">Odstranění dokumentu</span><span class="sxs-lookup"><span data-stu-id="1005c-124">Deleting a document</span></span>
* <span data-ttu-id="1005c-125">Odstraňování databáze aplikace hello</span><span class="sxs-lookup"><span data-stu-id="1005c-125">Deleting hello database</span></span>

<span data-ttu-id="1005c-126">Můžeme začít!</span><span class="sxs-lookup"><span data-stu-id="1005c-126">Now let's get started!</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1005c-127">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1005c-127">Prerequisites</span></span>
<span data-ttu-id="1005c-128">Ujistěte se, že máte hello následující:</span><span class="sxs-lookup"><span data-stu-id="1005c-128">Make sure you have hello following:</span></span>

* <span data-ttu-id="1005c-129">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="1005c-129">An active Azure account.</span></span> <span data-ttu-id="1005c-130">Pokud žádný nemáte, můžete si zaregistrovat [bezplatný účet](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="1005c-130">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span> <span data-ttu-id="1005c-131">Alternativně můžete použít hello [emulátoru DB Cosmos Azure](local-emulator.md) pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="1005c-131">Alternatively, you can use hello [Azure Cosmos DB Emulator](local-emulator.md) for this tutorial.</span></span>
* [<span data-ttu-id="1005c-132">Git</span><span class="sxs-lookup"><span data-stu-id="1005c-132">Git</span></span>](https://git-scm.com/downloads)
* <span data-ttu-id="1005c-133">[Java Development Kit (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html)</span><span class="sxs-lookup"><span data-stu-id="1005c-133">[Java Development Kit (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span></span>
* <span data-ttu-id="1005c-134">[Maven](http://maven.apache.org/download.cgi).</span><span class="sxs-lookup"><span data-stu-id="1005c-134">[Maven](http://maven.apache.org/download.cgi).</span></span>

## <a name="step-1-create-an-azure-cosmos-db-account"></a><span data-ttu-id="1005c-135">Krok 1: Vytvoření účtu služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="1005c-135">Step 1: Create an Azure Cosmos DB account</span></span>
<span data-ttu-id="1005c-136">Vytvořme účet služby Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="1005c-136">Let's create an Azure Cosmos DB account.</span></span> <span data-ttu-id="1005c-137">Pokud již máte účet, který chcete toouse, můžete přeskočit příliš[klon hello Githubu projektu](#GitClone).</span><span class="sxs-lookup"><span data-stu-id="1005c-137">If you already have an account you want toouse, you can skip ahead too[Clone hello GitHub project](#GitClone).</span></span> <span data-ttu-id="1005c-138">Pokud používáte hello emulátoru DB Cosmos Azure, postupujte podle kroků hello v [emulátoru DB Cosmos Azure](local-emulator.md) tooset až hello emulátoru a přeskočit příliš[klon hello Githubu projektu](#GitClone).</span><span class="sxs-lookup"><span data-stu-id="1005c-138">If you are using hello Azure Cosmos DB Emulator, follow hello steps at [Azure Cosmos DB Emulator](local-emulator.md) tooset up hello emulator and skip ahead too[Clone hello GitHub project](#GitClone).</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <span data-ttu-id="1005c-139"><a id="GitClone"></a>Krok 2: Klonování hello Githubu projektu</span><span class="sxs-lookup"><span data-stu-id="1005c-139"><a id="GitClone"></a>Step 2: Clone hello GitHub project</span></span>
<span data-ttu-id="1005c-140">Abyste mohli začít klonováním hello úložiště GitHub pro [Začínáme s Azure Cosmos DB a Java](https://github.com/Azure-Samples/documentdb-java-getting-started).</span><span class="sxs-lookup"><span data-stu-id="1005c-140">You can get started by cloning hello GitHub repository for [Get Started with Azure Cosmos DB and Java](https://github.com/Azure-Samples/documentdb-java-getting-started).</span></span> <span data-ttu-id="1005c-141">Například z místního adresáře spusťte hello následující tooretrieve hello ukázkový projekt místně.</span><span class="sxs-lookup"><span data-stu-id="1005c-141">For example, from a local directory run hello following tooretrieve hello sample project locally.</span></span>

    git clone git@github.com:Azure-Samples/azure-cosmos-db-documentdb-java-getting-started.git

    cd azure-cosmos-db-documentdb-java-getting-started

<span data-ttu-id="1005c-142">adresář Hello obsahuje `pom.xml` pro projekt hello a `src` složku obsahující Java zdrojového kódu včetně `Program.java` které ukazuje, jak provádějí jednoduché operace s Azure DB Cosmos jako je vytváření dokumentů a dotazování na data v rámci kolekce.</span><span class="sxs-lookup"><span data-stu-id="1005c-142">hello directory contains a `pom.xml` for hello project and a `src` folder containing Java source code including `Program.java` which shows how perform simple operations with Azure Cosmos DB like creating documents and querying data within a collection.</span></span> <span data-ttu-id="1005c-143">Hello `pom.xml` obsahuje závislost na hello [DocumentDB Java SDK na Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb).</span><span class="sxs-lookup"><span data-stu-id="1005c-143">hello `pom.xml` includes a dependency on hello [DocumentDB Java SDK on Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb).</span></span>

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-documentdb</artifactId>
        <version>LATEST</version>
    </dependency>

## <span data-ttu-id="1005c-144"><a id="Connect"></a>Krok 3: Připojení účtu Azure Cosmos DB tooan</span><span class="sxs-lookup"><span data-stu-id="1005c-144"><a id="Connect"></a>Step 3: Connect tooan Azure Cosmos DB account</span></span>
<span data-ttu-id="1005c-145">V dalším kroku head zpět toohello [portálu Azure](https://portal.azure.com) tooretrieve koncový bod a primární hlavní klíč.</span><span class="sxs-lookup"><span data-stu-id="1005c-145">Next, head back toohello [Azure Portal](https://portal.azure.com) tooretrieve your endpoint and primary master key.</span></span> <span data-ttu-id="1005c-146">Hello koncový bod Azure Cosmos DB a primární klíč jsou nezbytné, aby vaše aplikace toounderstand kde tooconnect a u Azure Cosmos DB tootrust připojení vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="1005c-146">hello Azure Cosmos DB endpoint and primary key are necessary for your application toounderstand where tooconnect to, and for Azure Cosmos DB tootrust your application's connection.</span></span>

<span data-ttu-id="1005c-147">V hello portálu Azure, přejděte tooyour Azure Cosmos DB účtu a pak klikněte na tlačítko **klíče**.</span><span class="sxs-lookup"><span data-stu-id="1005c-147">In hello Azure Portal, navigate tooyour Azure Cosmos DB account, and then click **Keys**.</span></span> <span data-ttu-id="1005c-148">Zkopírujte z portálu hello hello URI a vložte ji do `https://FILLME.documents.azure.com` v souboru Program.java hello.</span><span class="sxs-lookup"><span data-stu-id="1005c-148">Copy hello URI from hello portal and paste it into `https://FILLME.documents.azure.com` in hello Program.java file.</span></span> <span data-ttu-id="1005c-149">Kopírování hello primární klíč z portálu hello a vložte ji do `FILLME`.</span><span class="sxs-lookup"><span data-stu-id="1005c-149">Then copy hello PRIMARY KEY from hello portal and paste it into `FILLME`.</span></span>

    this.client = new DocumentClient(
        "https://FILLME.documents.azure.com",
        "FILLME"
        , new ConnectionPolicy(),
        ConsistencyLevel.Session);

![Snímek obrazovky hello používá hello toocreate kurzu NoSQL konzolovou aplikaci Java portálu Azure.][keys]

## <a name="step-4-create-a-database"></a><span data-ttu-id="1005c-152">Krok 4: Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="1005c-152">Step 4: Create a database</span></span>
<span data-ttu-id="1005c-153">Vaše Azure DB Cosmos [databáze](documentdb-resources.md#databases) lze vytvořit pomocí hello [metodu createDatabase](/java/api/com.microsoft.azure.documentdb._document_client.createdatabase) metoda hello **DocumentClient** třídy.</span><span class="sxs-lookup"><span data-stu-id="1005c-153">Your Azure Cosmos DB [database](documentdb-resources.md#databases) can be created by using hello [createDatabase](/java/api/com.microsoft.azure.documentdb._document_client.createdatabase) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="1005c-154">Databáze je logický kontejner úložiště dokumentů JSON rozděleného mezi kolekcemi hello.</span><span class="sxs-lookup"><span data-stu-id="1005c-154">A database is hello logical container of JSON document storage partitioned across collections.</span></span>

    Database database = new Database();
    database.setId("familydb");
    this.client.createDatabase(database, null);

## <span data-ttu-id="1005c-155"><a id="CreateColl"></a>Krok 5: Vytvoření kolekce</span><span class="sxs-lookup"><span data-stu-id="1005c-155"><a id="CreateColl"></a>Step 5: Create a collection</span></span>
> [!WARNING]
> <span data-ttu-id="1005c-156">**createCollection** vytvoří novou kolekci s vyhrazenou propustností, za kterou se hradí poplatky.</span><span class="sxs-lookup"><span data-stu-id="1005c-156">**createCollection** creates a new collection with reserved throughput, which has pricing implications.</span></span> <span data-ttu-id="1005c-157">Další podrobnosti najdete na [stránce s cenami](https://azure.microsoft.com/pricing/details/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="1005c-157">For more details, visit our [pricing page](https://azure.microsoft.com/pricing/details/cosmos-db/).</span></span>
> 
> 

<span data-ttu-id="1005c-158">A [kolekce](documentdb-resources.md#collections) lze vytvořit pomocí hello [createCollection](/java/api/com.microsoft.azure.documentdb._document_client.createcollection) metoda hello **DocumentClient** třídy.</span><span class="sxs-lookup"><span data-stu-id="1005c-158">A [collection](documentdb-resources.md#collections) can be created by using hello [createCollection](/java/api/com.microsoft.azure.documentdb._document_client.createcollection) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="1005c-159">Kolekce je kontejner dokumentů JSON a přidružené logiky javascriptové aplikace.</span><span class="sxs-lookup"><span data-stu-id="1005c-159">A collection is a container of JSON documents and associated JavaScript application logic.</span></span>


    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId("familycoll");

    // Azure Cosmos DB collections can be reserved with throughput specified in request units/second. 
    // Here we create a collection with 400 RU/s.
    RequestOptions requestOptions = new RequestOptions();
    requestOptions.setOfferThroughput(400);

    this.client.createCollection("/dbs/familydb", collectionInfo, requestOptions);

## <span data-ttu-id="1005c-160"><a id="CreateDoc"></a>Krok 6: Vytvoření dokumentů JSON</span><span class="sxs-lookup"><span data-stu-id="1005c-160"><a id="CreateDoc"></a>Step 6: Create JSON documents</span></span>
<span data-ttu-id="1005c-161">A [dokumentu](documentdb-resources.md#documents) lze vytvořit pomocí hello [createDocument](/java/api/com.microsoft.azure.documentdb._document_client.createdocument) metoda hello **DocumentClient** třídy.</span><span class="sxs-lookup"><span data-stu-id="1005c-161">A [document](documentdb-resources.md#documents) can be created by using hello [createDocument](/java/api/com.microsoft.azure.documentdb._document_client.createdocument) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="1005c-162">Dokumenty představují uživatelem definovaný (libovolný) obsah JSON.</span><span class="sxs-lookup"><span data-stu-id="1005c-162">Documents are user-defined (arbitrary) JSON content.</span></span> <span data-ttu-id="1005c-163">Nyní můžete vložit jeden nebo více dokumentů.</span><span class="sxs-lookup"><span data-stu-id="1005c-163">We can now insert one or more documents.</span></span> <span data-ttu-id="1005c-164">Pokud již máte data, které byste chtěli toostore v databázi, můžete použít Azure Cosmos DB [nástroj pro migraci dat](import-data.md) tooimport hello data do databáze.</span><span class="sxs-lookup"><span data-stu-id="1005c-164">If you already have data you'd like toostore in your database, you can use Azure Cosmos DB's [Data Migration tool](import-data.md) tooimport hello data into a database.</span></span>

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

![Diagram ilustrující hierarchický vztah hello mezi hello účet, hello online databáze, kolekce hello a hello dokumenty používanými v kurzu NoSQL toocreate konzolovou aplikaci Java hello](./media/documentdb-get-started/nosql-tutorial-account-database.png)

## <span data-ttu-id="1005c-166"><a id="Query"></a>Krok 7: Dotazování prostředků Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="1005c-166"><a id="Query"></a>Step 7: Query Azure Cosmos DB resources</span></span>
<span data-ttu-id="1005c-167">Azure Cosmos DB podporuje bohaté [dotazy](documentdb-sql-query.md) na dokumenty JSON uložené v každé z kolekcí.</span><span class="sxs-lookup"><span data-stu-id="1005c-167">Azure Cosmos DB supports rich [queries](documentdb-sql-query.md) against JSON documents stored in each collection.</span></span>  <span data-ttu-id="1005c-168">Hello následující vzorový kód ukazuje, jak tooquery dokumentů v Azure Cosmos DB pomocí syntaxe jazyka SQL s hello [dokumenty dotazu](/java/api/com.microsoft.azure.documentdb._document_client.querydocuments) metoda.</span><span class="sxs-lookup"><span data-stu-id="1005c-168">hello following sample code shows how tooquery documents in Azure Cosmos DB using SQL syntax with hello [queryDocuments](/java/api/com.microsoft.azure.documentdb._document_client.querydocuments) method.</span></span>

    FeedResponse<Document> queryResults = this.client.queryDocuments(
        "/dbs/familydb/colls/familycoll",
        "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", 
        null);

    System.out.println("Running SQL query...");
    for (Document family : queryResults.getQueryIterable()) {
        System.out.println(String.format("\tRead %s", family));
    }

## <span data-ttu-id="1005c-169"><a id="ReplaceDocument"></a>Krok 8: Nahrazení dokumentu JSON</span><span class="sxs-lookup"><span data-stu-id="1005c-169"><a id="ReplaceDocument"></a>Step 8: Replace JSON document</span></span>
<span data-ttu-id="1005c-170">Aktualizace dokumentů JSON pomocí hello podporuje Azure Cosmos DB [replaceDocument](/java/api/com.microsoft.azure.documentdb._document_client.replacedocument) metoda.</span><span class="sxs-lookup"><span data-stu-id="1005c-170">Azure Cosmos DB supports updating JSON documents using hello [replaceDocument](/java/api/com.microsoft.azure.documentdb._document_client.replacedocument) method.</span></span>

    // Update a property
    andersenFamily.Children[0].Grade = 6;

    this.client.replaceDocument(
        "/dbs/familydb/colls/familycoll/docs/Andersen.1", 
        andersenFamily,
        null);

## <span data-ttu-id="1005c-171"><a id="DeleteDocument"></a>Krok 9: Odstranění dokumentu JSON</span><span class="sxs-lookup"><span data-stu-id="1005c-171"><a id="DeleteDocument"></a>Step 9: Delete JSON document</span></span>
<span data-ttu-id="1005c-172">Podobně Azure Cosmos DB podporuje odstraňování dokumentů JSON pomocí hello [deleteDocument](/java/api/com.microsoft.azure.documentdb._document_client.deletedocument) metoda.</span><span class="sxs-lookup"><span data-stu-id="1005c-172">Similarly, Azure Cosmos DB supports deleting JSON documents using hello [deleteDocument](/java/api/com.microsoft.azure.documentdb._document_client.deletedocument) method.</span></span>  

    this.client.delete("/dbs/familydb/colls/familycoll/docs/Andersen.1", null);

## <span data-ttu-id="1005c-173"><a id="DeleteDatabase"></a>Krok 10: Odstranění databáze hello</span><span class="sxs-lookup"><span data-stu-id="1005c-173"><a id="DeleteDatabase"></a>Step 10: Delete hello database</span></span>
<span data-ttu-id="1005c-174">Odstraňování databáze hello vytvořili odebere hello databáze a všech jejích podřízených prostředků (kolekcí, dokumentů atd.).</span><span class="sxs-lookup"><span data-stu-id="1005c-174">Deleting hello created database removes hello database and all children resources (collections, documents, etc.).</span></span>

    this.client.deleteDatabase("/dbs/familydb", null);

## <span data-ttu-id="1005c-175"><a id="Run"></a>Krok 11: Spuštění celé konzolové aplikace jazyka Java!</span><span class="sxs-lookup"><span data-stu-id="1005c-175"><a id="Run"></a>Step 11: Run your Java console application all together!</span></span>
<span data-ttu-id="1005c-176">toorun hello aplikace z konzoly hello přejděte složky toohello projektu a kompilace pomocí nástroje Maven:</span><span class="sxs-lookup"><span data-stu-id="1005c-176">toorun hello application from hello console, navigate toohello project folder and compile using Maven:</span></span>
    
    mvn package

<span data-ttu-id="1005c-177">Spuštění `mvn package` stáhne nejnovější knihovny Azure Cosmos DB hello z Maven a vytváří `GetStarted-0.0.1-SNAPSHOT.jar`.</span><span class="sxs-lookup"><span data-stu-id="1005c-177">Running `mvn package` downloads hello latest Azure Cosmos DB library from Maven and produces `GetStarted-0.0.1-SNAPSHOT.jar`.</span></span> <span data-ttu-id="1005c-178">Spusťte aplikaci hello spuštěním:</span><span class="sxs-lookup"><span data-stu-id="1005c-178">Then run hello app by running:</span></span>

    mvn exec:java -D exec.mainClass=GetStarted.Program

<span data-ttu-id="1005c-179">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="1005c-179">Congratulations!</span></span> <span data-ttu-id="1005c-180">Dokončili jste tento kurz NoSQL a máte funkční konzolovou aplikaci jazyka Java!</span><span class="sxs-lookup"><span data-stu-id="1005c-180">You've completed this NoSQL tutorial and have a working Java console application!</span></span>

## <a name="next-steps"></a><span data-ttu-id="1005c-181">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1005c-181">Next steps</span></span>
* <span data-ttu-id="1005c-182">Chcete kurz vývoje webové aplikace v jazyce Java?</span><span class="sxs-lookup"><span data-stu-id="1005c-182">Want a Java web app tutorial?</span></span> <span data-ttu-id="1005c-183">Viz [Vytvoření webové aplikace pomocí Javy a služby Azure Cosmos DB](documentdb-java-application.md).</span><span class="sxs-lookup"><span data-stu-id="1005c-183">See [Build a web application with Java using Azure Cosmos DB](documentdb-java-application.md).</span></span>
* <span data-ttu-id="1005c-184">Zjistěte, jak příliš[monitorovat účet Azure Cosmos DB](monitor-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="1005c-184">Learn how too[monitor an Azure Cosmos DB account](monitor-accounts.md).</span></span>
* <span data-ttu-id="1005c-185">Spouštění dotazů na našem ukázkovou datovou sadu v hello [Query Playground](https://www.documentdb.com/sql/demo).</span><span class="sxs-lookup"><span data-stu-id="1005c-185">Run queries against our sample dataset in hello [Query Playground](https://www.documentdb.com/sql/demo).</span></span>
* <span data-ttu-id="1005c-186">Další informace o programovacím modelu hello hello vývoj části hello [stránce dokumentace Azure Cosmos DB](https://azure.microsoft.com/documentation/services/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="1005c-186">Learn more about hello programming model in hello Develop section of hello [Azure Cosmos DB documentation page](https://azure.microsoft.com/documentation/services/documentdb/).</span></span>

[keys]: media/documentdb-get-started/nosql-tutorial-keys.png
