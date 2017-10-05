---
title: "Databáze Azure Cosmos: Sestavení aplikace konzoly v jazyce Java s rozhraním API MongoDB | Dokumentace Microsoftu"
description: "Představuje ukázku kódu Java, kterou můžete použít k připojení a dotazování do rozhraní API MongoDB služby Azure Cosmos DB."
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: f84294d7d324f094d173f7a2ec89759266a74210
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cosmos-db-build-a-mongodb-api-console-app-with-java-and-the-azure-portal"></a><span data-ttu-id="b9e49-103">Databáze Azure Cosmos: Sestavení aplikace konzoly v jazyce Java s rozhraním API MongoDB na webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b9e49-103">Azure Cosmos DB: Build a MongoDB API console app with Java and the Azure portal</span></span>

<span data-ttu-id="b9e49-104">Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku.</span><span class="sxs-lookup"><span data-stu-id="b9e49-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="b9e49-105">Můžete snadno vytvořit a dotazovat databáze dotazů, klíčů/hodnot a grafů, které tak můžou využívat výhody použitelnosti v celosvětovém měřítku a možností horizontálního škálování v jádru Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="b9e49-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from the global distribution and horizontal scale capabilities at the core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="b9e49-106">Tento rychlý start popisuje způsob vytvoření účtu databáze Azure Cosmos, databáze dokumentů a kolekce pomocí webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="b9e49-106">This quick start demonstrates how to create an Azure Cosmos DB account, document database, and collection using the Azure portal.</span></span> <span data-ttu-id="b9e49-107">Potom sestavíte a nasadíte aplikaci konzoly založenou na [ovladači Java MongoDB](https://docs.mongodb.com/ecosystem/drivers/java/).</span><span class="sxs-lookup"><span data-stu-id="b9e49-107">You'll then build and deploy a console app built on the [MongoDB Java driver](https://docs.mongodb.com/ecosystem/drivers/java/).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="b9e49-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b9e49-108">Prerequisites</span></span>

* <span data-ttu-id="b9e49-109">Než budete moct tuto ukázku spustit, je potřeba splnit následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="b9e49-109">Before you can run this sample, you must have the following prerequisites:</span></span>
   * <span data-ttu-id="b9e49-110">JDK 1.7+ (pokud nemáte JDK, spusťte `apt-get install default-jdk`)</span><span class="sxs-lookup"><span data-stu-id="b9e49-110">JDK 1.7+ (Run `apt-get install default-jdk` if you don't have JDK)</span></span>
   * <span data-ttu-id="b9e49-111">Maven (pokud nemáte Maven, spusťte `apt-get install maven`)</span><span class="sxs-lookup"><span data-stu-id="b9e49-111">Maven (Run `apt-get install maven` if you don't have Maven)</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="b9e49-112">Vytvoření účtu databáze</span><span class="sxs-lookup"><span data-stu-id="b9e49-112">Create a database account</span></span>

[!INCLUDE [mongodb-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="add-a-collection"></a><span data-ttu-id="b9e49-113">Přidání kolekce</span><span class="sxs-lookup"><span data-stu-id="b9e49-113">Add a collection</span></span>

<span data-ttu-id="b9e49-114">Novou databázi pojmenujte **db** a novou kolekci **coll**.</span><span class="sxs-lookup"><span data-stu-id="b9e49-114">Name your new database, **db**, and your new collection, **coll**.</span></span>

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="clone-the-sample-application"></a><span data-ttu-id="b9e49-115">Klonování ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="b9e49-115">Clone the sample application</span></span>

<span data-ttu-id="b9e49-116">Teď naklonujeme aplikaci rozhraní API MongoDB z GitHubu, nastavíme připojovací řetězec a spustíme ji.</span><span class="sxs-lookup"><span data-stu-id="b9e49-116">Now let's clone a MongoDB API app from github, set the connection string, and run it.</span></span> <span data-ttu-id="b9e49-117">Přesvědčíte se, jak snadno se pracuje s daty prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="b9e49-117">You'll see how easy it is to work with data programmatically.</span></span> 

1. <span data-ttu-id="b9e49-118">Otevřete okno terminálu Git, jako je třeba Git Bash, a pomocí `cd` přejděte do pracovního adresáře.</span><span class="sxs-lookup"><span data-stu-id="b9e49-118">Open a git terminal window, such as git bash, and `cd` to a working directory.</span></span>  

2. <span data-ttu-id="b9e49-119">Ukázkové úložiště naklonujete spuštěním následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="b9e49-119">Run the following command to clone the sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-getting-started.git
    ```

3. <span data-ttu-id="b9e49-120">Potom otevřete soubor řešení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b9e49-120">Then open the solution file in Visual Studio.</span></span> 

## <a name="review-the-code"></a><span data-ttu-id="b9e49-121">Kontrola kódu</span><span class="sxs-lookup"><span data-stu-id="b9e49-121">Review the code</span></span>

<span data-ttu-id="b9e49-122">Ještě jednou se stručně podívejme na to, co se v aplikaci děje.</span><span class="sxs-lookup"><span data-stu-id="b9e49-122">Let's make a quick review of what's happening in the app.</span></span> <span data-ttu-id="b9e49-123">Otevřete soubor `Program.cs` a zjistíte, že tyto řádky kódu vytvářejí prostředky databáze Azure Cosmos.</span><span class="sxs-lookup"><span data-stu-id="b9e49-123">Open the `Program.cs` file and you'll find that these lines of code create the Azure Cosmos DB resources.</span></span> 

* <span data-ttu-id="b9e49-124">Inicializuje se DocumentClient.</span><span class="sxs-lookup"><span data-stu-id="b9e49-124">The DocumentClient is initialized.</span></span>

    ```java
    MongoClientURI uri = new MongoClientURI("FILLME");`

    MongoClient mongoClient = new MongoClient(uri);            
    ```

* <span data-ttu-id="b9e49-125">Vytvoří se nová databáze a kolekce.</span><span class="sxs-lookup"><span data-stu-id="b9e49-125">A new database and collection are created.</span></span>

    ```java
    MongoDatabase database = mongoClient.getDatabase("db");

    MongoCollection<Document> collection = database.getCollection("coll");
    ```

* <span data-ttu-id="b9e49-126">Pomocí metody `MongoCollection.insertOne` se vloží některé dokumenty.</span><span class="sxs-lookup"><span data-stu-id="b9e49-126">Some documents are inserted using `MongoCollection.insertOne`</span></span>

    ```java
    Document document = new Document("fruit", "apple")
    collection.insertOne(document);
    ```

* <span data-ttu-id="b9e49-127">Pomocí metody `MongoCollection.find` se provedou některé dotazy.</span><span class="sxs-lookup"><span data-stu-id="b9e49-127">Some queries are performed using `MongoCollection.find`</span></span>

    ```java
    Document queryResult = collection.find(Filters.eq("fruit", "apple")).first();
    System.out.println(queryResult.toJson());       
    ```

## <a name="update-your-connection-string"></a><span data-ttu-id="b9e49-128">Aktualizace připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="b9e49-128">Update your connection string</span></span>

<span data-ttu-id="b9e49-129">Teď se vraťte zpátky na web Azure Portal, kde najdete informace o připojovacím řetězci, a zkopírujte je do aplikace.</span><span class="sxs-lookup"><span data-stu-id="b9e49-129">Now go back to the Azure portal to get your connection string information and copy it into the app.</span></span>

1. <span data-ttu-id="b9e49-130">V účtu vyberte možnost **Rychlý start**, vyberte jazyk Java a potom zkopírujte připojovací řetězec do schránky.</span><span class="sxs-lookup"><span data-stu-id="b9e49-130">From the Account, select **Quick Start**, select Java, then copy the connection string to your clipboard</span></span>

2. <span data-ttu-id="b9e49-131">Otevřete soubor `Program.java` a nahraďte argument konstruktoru MongoClientURI připojovacím řetězcem.</span><span class="sxs-lookup"><span data-stu-id="b9e49-131">Open the `Program.java` file, replace the argument to the MongoClientURI constructor with the connection string.</span></span> <span data-ttu-id="b9e49-132">Teď jste aktualizovali aplikaci a zadali do ní všechny informace potřebné ke komunikaci s databází Azure Cosmos.</span><span class="sxs-lookup"><span data-stu-id="b9e49-132">You've now updated your app with all the info it needs to communicate with Azure Cosmos DB.</span></span> 
    
## <a name="run-the-console-app"></a><span data-ttu-id="b9e49-133">Spuštění aplikace konzoly</span><span class="sxs-lookup"><span data-stu-id="b9e49-133">Run the console app</span></span>

1. <span data-ttu-id="b9e49-134">Spusťte v terminálu `mvn package`, aby se nainstalovaly požadované moduly NPM.</span><span class="sxs-lookup"><span data-stu-id="b9e49-134">Run `mvn package` in a terminal to install required npm modules</span></span>

2. <span data-ttu-id="b9e49-135">Spuštění v terminálu `mvn exec:java -D exec.mainClass=GetStarted.Program`, aby se spustila aplikace Java.</span><span class="sxs-lookup"><span data-stu-id="b9e49-135">Run `mvn exec:java -D exec.mainClass=GetStarted.Program` in a terminal to start your Java application.</span></span>

<span data-ttu-id="b9e49-136">Teď můžete provádět dotazy a úpravy a pracovat s těmito novými daty v prostředí použít [Robomongo](mongodb-robomongo.md) / [Studio 3T](mongodb-mongochef.md).</span><span class="sxs-lookup"><span data-stu-id="b9e49-136">You can now use [Robomongo](mongodb-robomongo.md) / [Studio 3T](mongodb-mongochef.md) to query, modify, and work with this new data.</span></span>

## <a name="review-slas-in-the-azure-portal"></a><span data-ttu-id="b9e49-137">Ověření smluv SLA na webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b9e49-137">Review SLAs in the Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="b9e49-138">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="b9e49-138">Clean up resources</span></span>

<span data-ttu-id="b9e49-139">Pokud nebudete tuto aplikace nadále používat, odstraňte na základě následujícího postupu z portálu Azure Portal všechny prostředky vytvořené podle tohoto rychlého startu:</span><span class="sxs-lookup"><span data-stu-id="b9e49-139">If you're not going to continue to use this app, delete all resources created by this quickstart in the Azure portal with the following steps:</span></span>

1. <span data-ttu-id="b9e49-140">V nabídce vlevo na portálu Azure Portal klikněte na **Skupiny prostředků** a pak klikněte na název vytvořeného prostředku.</span><span class="sxs-lookup"><span data-stu-id="b9e49-140">From the left-hand menu in the Azure portal, click **Resource groups** and then click the name of the resource you created.</span></span> 
2. <span data-ttu-id="b9e49-141">Na stránce skupiny prostředků klikněte na **Odstranit**, do textového pole zadejte prostředek, který chcete odstranit, a pak klikněte na **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="b9e49-141">On your resource group page, click **Delete**, type the name of the resource to delete in the text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b9e49-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b9e49-142">Next steps</span></span>

<span data-ttu-id="b9e49-143">V tomto rychlém startu jste se seznámili s postupem vytvoření účtu služby Azure Cosmos DB, vytvoření kolekce pomocí Průzkumníka dat a spuštění aplikace konzoly.</span><span class="sxs-lookup"><span data-stu-id="b9e49-143">In this quickstart, you've learned how to create an Azure Cosmos DB account, create a collection using the Data Explorer, and run a console app.</span></span> <span data-ttu-id="b9e49-144">Teď můžete do účtu databáze Cosmos importovat další data.</span><span class="sxs-lookup"><span data-stu-id="b9e49-144">You can now import additional data to your Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="b9e49-145">Importování dat MongoDB do databáze Azure Cosmos</span><span class="sxs-lookup"><span data-stu-id="b9e49-145">Import MongoDB data into Azure Cosmos DB</span></span>](mongodb-migrate.md)


