---
title: "Azure Cosmos DB: Vytvoření konzolové aplikace v jazyce Java a hello MongoDB API | Microsoft Docs"
description: "Uvede ukázku kódu Java pomocí dotazu tooand tooconnect hello rozhraní API služby Azure Cosmos DB MongoDB"
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
ms.openlocfilehash: fbe416f6b20ed2bb83a1d41eb70ffc6e3cee2b61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-mongodb-api-console-app-with-java-and-hello-azure-portal"></a><span data-ttu-id="cdda3-103">Azure Cosmos DB: Sestavení aplikace konzoly MongoDB rozhraní API s Java a hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="cdda3-103">Azure Cosmos DB: Build a MongoDB API console app with Java and hello Azure portal</span></span>

<span data-ttu-id="cdda3-104">Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku.</span><span class="sxs-lookup"><span data-stu-id="cdda3-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="cdda3-105">Můžete rychle vytvořit a dotazovat dokumentu, klíč/hodnota a graf databází, které těžit z globální distribuční hello a možnosti vodorovné škálování jádrem hello Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="cdda3-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="cdda3-106">Tento rychlý start předvádí, jak hello toocreate účet Azure Cosmos DB, dokumentu databáze a kolekce pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="cdda3-106">This quick start demonstrates how toocreate an Azure Cosmos DB account, document database, and collection using hello Azure portal.</span></span> <span data-ttu-id="cdda3-107">Budete pak sestavení a nasazení aplikace konzoly založený na hello [MongoDB Java ovladač](https://docs.mongodb.com/ecosystem/drivers/java/).</span><span class="sxs-lookup"><span data-stu-id="cdda3-107">You'll then build and deploy a console app built on hello [MongoDB Java driver](https://docs.mongodb.com/ecosystem/drivers/java/).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="cdda3-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cdda3-108">Prerequisites</span></span>

* <span data-ttu-id="cdda3-109">Před spuštěním této ukázce, musíte mít hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="cdda3-109">Before you can run this sample, you must have hello following prerequisites:</span></span>
   * <span data-ttu-id="cdda3-110">JDK 1.7+ (pokud nemáte JDK, spusťte `apt-get install default-jdk`)</span><span class="sxs-lookup"><span data-stu-id="cdda3-110">JDK 1.7+ (Run `apt-get install default-jdk` if you don't have JDK)</span></span>
   * <span data-ttu-id="cdda3-111">Maven (pokud nemáte Maven, spusťte `apt-get install maven`)</span><span class="sxs-lookup"><span data-stu-id="cdda3-111">Maven (Run `apt-get install maven` if you don't have Maven)</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="cdda3-112">Vytvoření účtu databáze</span><span class="sxs-lookup"><span data-stu-id="cdda3-112">Create a database account</span></span>

[!INCLUDE [mongodb-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="add-a-collection"></a><span data-ttu-id="cdda3-113">Přidání kolekce</span><span class="sxs-lookup"><span data-stu-id="cdda3-113">Add a collection</span></span>

<span data-ttu-id="cdda3-114">Novou databázi pojmenujte **db** a novou kolekci **coll**.</span><span class="sxs-lookup"><span data-stu-id="cdda3-114">Name your new database, **db**, and your new collection, **coll**.</span></span>

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="clone-hello-sample-application"></a><span data-ttu-id="cdda3-115">Klonování hello ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="cdda3-115">Clone hello sample application</span></span>

<span data-ttu-id="cdda3-116">Teď umožňuje nastavit připojovací řetězec hello klonování MongoDB API aplikace z githubu a potom ho spusťte.</span><span class="sxs-lookup"><span data-stu-id="cdda3-116">Now let's clone a MongoDB API app from github, set hello connection string, and run it.</span></span> <span data-ttu-id="cdda3-117">Uvidíte, jak je snadné toowork s daty prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="cdda3-117">You'll see how easy it is toowork with data programmatically.</span></span> 

1. <span data-ttu-id="cdda3-118">Otevřete okno terminálu git, jako je například git bash a `cd` tooa pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="cdda3-118">Open a git terminal window, such as git bash, and `cd` tooa working directory.</span></span>  

2. <span data-ttu-id="cdda3-119">Spusťte následující příkaz tooclone hello Ukázka úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="cdda3-119">Run hello following command tooclone hello sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-getting-started.git
    ```

3. <span data-ttu-id="cdda3-120">Poté otevřete soubor řešení hello v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cdda3-120">Then open hello solution file in Visual Studio.</span></span> 

## <a name="review-hello-code"></a><span data-ttu-id="cdda3-121">Zkontrolujte hello kódu</span><span class="sxs-lookup"><span data-stu-id="cdda3-121">Review hello code</span></span>

<span data-ttu-id="cdda3-122">Provedeme jejich stručný přehled o dění v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="cdda3-122">Let's make a quick review of what's happening in hello app.</span></span> <span data-ttu-id="cdda3-123">Otevřete hello `Program.cs` souboru a najdete, že tyto řádky kódu vytvořit hello prostředky Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="cdda3-123">Open hello `Program.cs` file and you'll find that these lines of code create hello Azure Cosmos DB resources.</span></span> 

* <span data-ttu-id="cdda3-124">Hello DocumentClient je inicializován.</span><span class="sxs-lookup"><span data-stu-id="cdda3-124">hello DocumentClient is initialized.</span></span>

    ```java
    MongoClientURI uri = new MongoClientURI("FILLME");`

    MongoClient mongoClient = new MongoClient(uri);            
    ```

* <span data-ttu-id="cdda3-125">Vytvoří se nová databáze a kolekce.</span><span class="sxs-lookup"><span data-stu-id="cdda3-125">A new database and collection are created.</span></span>

    ```java
    MongoDatabase database = mongoClient.getDatabase("db");

    MongoCollection<Document> collection = database.getCollection("coll");
    ```

* <span data-ttu-id="cdda3-126">Pomocí metody `MongoCollection.insertOne` se vloží některé dokumenty.</span><span class="sxs-lookup"><span data-stu-id="cdda3-126">Some documents are inserted using `MongoCollection.insertOne`</span></span>

    ```java
    Document document = new Document("fruit", "apple")
    collection.insertOne(document);
    ```

* <span data-ttu-id="cdda3-127">Pomocí metody `MongoCollection.find` se provedou některé dotazy.</span><span class="sxs-lookup"><span data-stu-id="cdda3-127">Some queries are performed using `MongoCollection.find`</span></span>

    ```java
    Document queryResult = collection.find(Filters.eq("fruit", "apple")).first();
    System.out.println(queryResult.toJson());       
    ```

## <a name="update-your-connection-string"></a><span data-ttu-id="cdda3-128">Aktualizace připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="cdda3-128">Update your connection string</span></span>

<span data-ttu-id="cdda3-129">Nyní přejděte zpět toohello Azure portálu tooget vaše informace o připojovacím řetězci a zkopírujte jej do aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="cdda3-129">Now go back toohello Azure portal tooget your connection string information and copy it into hello app.</span></span>

1. <span data-ttu-id="cdda3-130">Hello účtu, vyberte **rychlý Start**vyberte Java a pak zkopírujte hello připojovací řetězec tooyour schránky</span><span class="sxs-lookup"><span data-stu-id="cdda3-130">From hello Account, select **Quick Start**, select Java, then copy hello connection string tooyour clipboard</span></span>

2. <span data-ttu-id="cdda3-131">Otevřete hello `Program.java` souboru, nahraďte hello argument toohello MongoClientURI konstruktor hello připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="cdda3-131">Open hello `Program.java` file, replace hello argument toohello MongoClientURI constructor with hello connection string.</span></span> <span data-ttu-id="cdda3-132">Jste nyní aktualizovat vaši aplikaci s všechny údaje hello potřebuje toocommunicate s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="cdda3-132">You've now updated your app with all hello info it needs toocommunicate with Azure Cosmos DB.</span></span> 
    
## <a name="run-hello-console-app"></a><span data-ttu-id="cdda3-133">Spusťte konzolovou aplikaci hello</span><span class="sxs-lookup"><span data-stu-id="cdda3-133">Run hello console app</span></span>

1. <span data-ttu-id="cdda3-134">Spustit `mvn package` v terminálu tooinstall požadované moduly npm</span><span class="sxs-lookup"><span data-stu-id="cdda3-134">Run `mvn package` in a terminal tooinstall required npm modules</span></span>

2. <span data-ttu-id="cdda3-135">Spustit `mvn exec:java -D exec.mainClass=GetStarted.Program` v terminálu toostart aplikace v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="cdda3-135">Run `mvn exec:java -D exec.mainClass=GetStarted.Program` in a terminal toostart your Java application.</span></span>

<span data-ttu-id="cdda3-136">Teď můžete použít [Robomongo](mongodb-robomongo.md) / [Studio 3T](mongodb-mongochef.md) tooquery, upravit a pracovat s Tato nová data.</span><span class="sxs-lookup"><span data-stu-id="cdda3-136">You can now use [Robomongo](mongodb-robomongo.md) / [Studio 3T](mongodb-mongochef.md) tooquery, modify, and work with this new data.</span></span>

## <a name="review-slas-in-hello-azure-portal"></a><span data-ttu-id="cdda3-137">Zkontrolujte SLA v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="cdda3-137">Review SLAs in hello Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="cdda3-138">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="cdda3-138">Clean up resources</span></span>

<span data-ttu-id="cdda3-139">Pokud ale nebudete toocontinue toouse této aplikace, odstraňte všechny prostředky, které jsou vytvořené tento rychlý start v hello portál Azure s hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cdda3-139">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span>

1. <span data-ttu-id="cdda3-140">V levé nabídce hello v hello portálu Azure klikněte na **skupiny prostředků** a pak klikněte na název hello hello prostředků, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="cdda3-140">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="cdda3-141">Na stránce skupiny prostředků, klikněte na tlačítko **odstranit**hello textového pole zadejte název hello toodelete hello prostředků a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="cdda3-141">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cdda3-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cdda3-142">Next steps</span></span>

<span data-ttu-id="cdda3-143">V tento rychlý start když jste se naučili jak toocreate účtu Azure Cosmos DB, vytvořte kolekci pomocí hello Průzkumníku dat a spusťte konzolovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cdda3-143">In this quickstart, you've learned how toocreate an Azure Cosmos DB account, create a collection using hello Data Explorer, and run a console app.</span></span> <span data-ttu-id="cdda3-144">Nyní můžete importovat další data tooyour Cosmos DB účtu.</span><span class="sxs-lookup"><span data-stu-id="cdda3-144">You can now import additional data tooyour Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="cdda3-145">Importování dat MongoDB do služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="cdda3-145">Import MongoDB data into Azure Cosmos DB</span></span>](mongodb-migrate.md)


