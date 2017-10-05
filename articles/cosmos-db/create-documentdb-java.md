---
title: "Vytvoření databáze dokumentů Azure Cosmos DB pomocí Javy | Dokumentace Microsoftu"
description: "Představuje ukázku kódu Java, který můžete použít k připojení a dotazování do rozhraní API DocumentDB služby Azure Cosmos DB"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 89ea62bb-c620-46d5-baa0-eefd9888557c
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: hero-article
ms.date: 08/02/2017
ms.author: mimig
ms.openlocfilehash: df1a25d703a7b8082bdabb4f7d593cb005d416fe
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-cosmos-db-create-a-document-database-using-java-and-the-azure-portal"></a><span data-ttu-id="a4c06-103">Azure Cosmos DB: Vytvoření databáze dokumentů pomocí Javy a webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a4c06-103">Azure Cosmos DB: Create a document database using Java and the Azure portal</span></span>

<span data-ttu-id="a4c06-104">Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku.</span><span class="sxs-lookup"><span data-stu-id="a4c06-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="a4c06-105">Můžete snadno vytvořit a dotazovat databáze dotazů, klíčů/hodnot a grafů, které tak můžou využívat výhody použitelnosti v celosvětovém měřítku a možností horizontálního škálování v jádru databáze Azure Cosmos.</span><span class="sxs-lookup"><span data-stu-id="a4c06-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from the global distribution and horizontal scale capabilities at the core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="a4c06-106">V tomto rychlém startu se vytvoří databáze dokumentů pomocí nástrojů pro Azure Cosmos DB na webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="a4c06-106">This quickstart creates a document database using the Azure portal tools for Azure Cosmos DB.</span></span> <span data-ttu-id="a4c06-107">V tomto rychlém startu se také dozvíte, jak rychle vytvořit konzolovou aplikaci Java pomocí [rozhraní API Java DocumentDB](documentdb-sdk-java.md).</span><span class="sxs-lookup"><span data-stu-id="a4c06-107">This quickstart also shows you how to quickly create a Java console app using the [DocumentDB Java API](documentdb-sdk-java.md).</span></span> <span data-ttu-id="a4c06-108">Pokyny v tomto rychlém startu platí pro všechny operační systémy, které podporují Javu.</span><span class="sxs-lookup"><span data-stu-id="a4c06-108">The instructions in this quickstart can be followed on any operating system that is capable of running Java.</span></span> <span data-ttu-id="a4c06-109">Po dokončení tohoto rychlého startu budete vědět, jak vytvořit a upravit prostředky databáze dokumentů v uživatelském rozhraní nebo programově podle toho, čemu dáváte přednost.</span><span class="sxs-lookup"><span data-stu-id="a4c06-109">By completing this quickstart you'll be familiar with creating and modifying document database resources in either the UI or programatically, whichever is your preference.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a4c06-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a4c06-110">Prerequisites</span></span>

* [<span data-ttu-id="a4c06-111">Java Development Kit (JDK) 1.7+</span><span class="sxs-lookup"><span data-stu-id="a4c06-111">Java Development Kit (JDK) 1.7+</span></span>](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
    * <span data-ttu-id="a4c06-112">Na Ubuntu nainstalujte sadu JDK spuštěním příkazu `apt-get install default-jdk`.</span><span class="sxs-lookup"><span data-stu-id="a4c06-112">On Ubuntu, run `apt-get install default-jdk` to install the JDK.</span></span>
    * <span data-ttu-id="a4c06-113">Nezapomeňte nastavit proměnnou prostředí JAVA_HOME tak, aby odkazovala na složku, ve které je sada JDK nainstalovaná.</span><span class="sxs-lookup"><span data-stu-id="a4c06-113">Be sure to set the JAVA_HOME environment variable to point to the folder where the JDK is installed.</span></span>
* <span data-ttu-id="a4c06-114">[Stáhněte](http://maven.apache.org/download.cgi) a [nainstalujte](http://maven.apache.org/install.html) binární archiv [Maven](http://maven.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="a4c06-114">[Download](http://maven.apache.org/download.cgi) and [install](http://maven.apache.org/install.html) a [Maven](http://maven.apache.org/) binary archive</span></span>
    * <span data-ttu-id="a4c06-115">Na Ubuntu můžete Maven nainstalovat spuštěním příkazu `apt-get install maven`.</span><span class="sxs-lookup"><span data-stu-id="a4c06-115">On Ubuntu, you can run `apt-get install maven` to install Maven.</span></span>
* [<span data-ttu-id="a4c06-116">Git</span><span class="sxs-lookup"><span data-stu-id="a4c06-116">Git</span></span>](https://www.git-scm.com/)
    * <span data-ttu-id="a4c06-117">Na Ubuntu můžete Git nainstalovat spuštěním příkazu `sudo apt-get install git`.</span><span class="sxs-lookup"><span data-stu-id="a4c06-117">On Ubuntu, you can run `sudo apt-get install git` to install Git.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="a4c06-118">Vytvoření účtu databáze</span><span class="sxs-lookup"><span data-stu-id="a4c06-118">Create a database account</span></span>

<span data-ttu-id="a4c06-119">Než budete moct vytvořit databázi dokumentů, je potřeba pomocí služby Azure Cosmos DB vytvořit účet databáze SQL (DocumentDB).</span><span class="sxs-lookup"><span data-stu-id="a4c06-119">Before you can create a document database, you need to create a SQL (DocumentDB) database account with Azure Cosmos DB.</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a><span data-ttu-id="a4c06-120">Přidání kolekce</span><span class="sxs-lookup"><span data-stu-id="a4c06-120">Add a collection</span></span>

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="add-sample-data"></a>
## <a name="add-sample-data"></a><span data-ttu-id="a4c06-121">Přidání ukázkových dat</span><span class="sxs-lookup"><span data-stu-id="a4c06-121">Add sample data</span></span>

<span data-ttu-id="a4c06-122">Teď můžete do nové kolekce přidávat data pomocí Průzkumníka dat.</span><span class="sxs-lookup"><span data-stu-id="a4c06-122">You can now add data to your new collection using Data Explorer.</span></span>

1. <span data-ttu-id="a4c06-123">V Průzkumníku dat se nová databáze zobrazí v podokně Kolekce.</span><span class="sxs-lookup"><span data-stu-id="a4c06-123">In Data Explorer, the new database appears in the Collections pane.</span></span> <span data-ttu-id="a4c06-124">Rozbalte databázi **Tasks**, rozbalte kolekci **Items**, klikněte na **Dokumenty** a potom klikněte na **Nové dokumenty**.</span><span class="sxs-lookup"><span data-stu-id="a4c06-124">Expand the **Tasks** database, expand the **Items** collection, click **Documents**, and then click **New Documents**.</span></span> 

   ![Vytváření nových dokumentů v Průzkumníku dat na portálu Azure Portal](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-new-document.png)
  
2. <span data-ttu-id="a4c06-126">Teď do kolekce přidejte dokument s následující strukturou.</span><span class="sxs-lookup"><span data-stu-id="a4c06-126">Now add a document to the collection with the following structure.</span></span>

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries.",
         "isComplete": false
     }
     ```

3. <span data-ttu-id="a4c06-127">Po přidání formátu json na kartu **Dokumenty** klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="a4c06-127">Once you've added the json to the **Documents** tab, click **Save**.</span></span>

    ![Zkopírujte data json a v Průzkumníku dat na webu Azure Portal klikněte na Uložit.](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-save-document.png)

4.  <span data-ttu-id="a4c06-129">Vytvořte a uložte ještě jeden dokument, ve kterém vložíte jedinečnou hodnotu pro vlastnost `id` a změníte ostatní vlastnosti podle svých potřeb.</span><span class="sxs-lookup"><span data-stu-id="a4c06-129">Create and save one more document where you insert a unique value for the `id` property, and change the other properties as you see fit.</span></span> <span data-ttu-id="a4c06-130">Nové dokumenty můžou mít jakoukoli strukturu, protože Azure Cosmos DB neuplatňuje pro data žádné schéma.</span><span class="sxs-lookup"><span data-stu-id="a4c06-130">Your new documents can have any structure you want as Azure Cosmos DB doesn't impose any schema on your data.</span></span>

     <span data-ttu-id="a4c06-131">Teď můžete svá data načíst pomocí dotazů v Průzkumníku dat kliknutím na tlačítka **Upravit filtr** a **Použít filtr**.</span><span class="sxs-lookup"><span data-stu-id="a4c06-131">You can now use queries in Data Explorer to retrieve your data by clicking the **Edit Filter** and **Apply Filter** buttons.</span></span> <span data-ttu-id="a4c06-132">Ve výchozím nastavení používá Průzkumník dat pro načtení všech dokumentů v kolekci příkaz `SELECT * FROM c`. Ten však můžete změnit na jiný [příkaz jazyka SQL](documentdb-sql-query.md), například `SELECT * FROM c ORDER BY c._ts DESC`, který vrátí všechny dokumenty v sestupném pořadí podle časového razítka.</span><span class="sxs-lookup"><span data-stu-id="a4c06-132">By default, Data Explorer uses `SELECT * FROM c` to retrieve all documents in the collection, but you can change that to a different [SQL query](documentdb-sql-query.md), such as `SELECT * FROM c ORDER BY c._ts DESC`, to return all the documents in descending order based on their timestamp.</span></span> 
 
     <span data-ttu-id="a4c06-133">Průzkumník dat můžete použít také pro vytváření uložených procedur, funkcí UDF a triggerů pro provádění obchodní logiky a také propustnosti škálování na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="a4c06-133">You can also use Data Explorer to create stored procedures, UDFs, and triggers to perform server-side business logic as well as scale throughput.</span></span> <span data-ttu-id="a4c06-134">Průzkumník dat zpřístupní všechna integrovaná programová data v rozhraních API, ale zajistí jednoduchý přístup k vašim datům na portálu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="a4c06-134">Data Explorer exposes all of the built-in programmatic data access available in the APIs, but provides easy access to your data in the Azure portal.</span></span>

## <a name="clone-the-sample-application"></a><span data-ttu-id="a4c06-135">Klonování ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="a4c06-135">Clone the sample application</span></span>

<span data-ttu-id="a4c06-136">Teď přejděme k práci s kódem.</span><span class="sxs-lookup"><span data-stu-id="a4c06-136">Now let's switch to working with code.</span></span> <span data-ttu-id="a4c06-137">Naklonujeme aplikaci s rozhraním API DocumentDB z GitHubu, nastavíme připojovací řetězec a spustíme ji.</span><span class="sxs-lookup"><span data-stu-id="a4c06-137">Let's clone a DocumentDB API app from GitHub, set the connection string, and run it.</span></span> <span data-ttu-id="a4c06-138">Uvidíte, jak snadno se pracuje s daty prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="a4c06-138">You see how easy it is to work with data programmatically.</span></span> 

1. <span data-ttu-id="a4c06-139">Otevřete okno terminálu Git, jako je třeba Git Bash, a pomocí `CD` přejděte do pracovního adresáře.</span><span class="sxs-lookup"><span data-stu-id="a4c06-139">Open a git terminal window, such as git bash, and `CD` to a working directory.</span></span>  

2. <span data-ttu-id="a4c06-140">Ukázkové úložiště naklonujete spuštěním následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="a4c06-140">Run the following command to clone the sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-java-getting-started.git
    ```

## <a name="review-the-code"></a><span data-ttu-id="a4c06-141">Kontrola kódu</span><span class="sxs-lookup"><span data-stu-id="a4c06-141">Review the code</span></span>

<span data-ttu-id="a4c06-142">Ještě jednou se stručně podívejme na to, co se v aplikaci děje.</span><span class="sxs-lookup"><span data-stu-id="a4c06-142">Let's make a quick review of what's happening in the app.</span></span> <span data-ttu-id="a4c06-143">Otevřete soubor `Program.java` ze složky \src\GetStarted a vyhledejte tyto řádky kódu, které vytvářejí prostředky služby Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="a4c06-143">Open the `Program.java` file from the \src\GetStarted folder, and find these lines of code that create the Azure Cosmos DB resources.</span></span> 

* <span data-ttu-id="a4c06-144">Inicializuje se `DocumentClient`.</span><span class="sxs-lookup"><span data-stu-id="a4c06-144">The `DocumentClient` is initialized.</span></span>

    ```java
    this.client = new DocumentClient("https://FILLME.documents.azure.com",
            "FILLME", 
            new ConnectionPolicy(),
            ConsistencyLevel.Session);
    ```

* <span data-ttu-id="a4c06-145">Vytvoří se nová databáze.</span><span class="sxs-lookup"><span data-stu-id="a4c06-145">A new database is created.</span></span>

    ```java
    Database database = new Database();
    database.setId(databaseName);
    
    this.client.createDatabase(database, null);
    ```

* <span data-ttu-id="a4c06-146">Vytvoří se nová kolekce.</span><span class="sxs-lookup"><span data-stu-id="a4c06-146">A new collection is created.</span></span>

    ```java
    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId(collectionName);

    ...

    this.client.createCollection(databaseLink, collectionInfo, requestOptions);
    ```

* <span data-ttu-id="a4c06-147">Vytvoří se některé dokumenty.</span><span class="sxs-lookup"><span data-stu-id="a4c06-147">Some documents are created.</span></span>

    ```java
    // Any Java object within your code can be serialized into JSON and written to Azure Cosmos DB
    Family andersenFamily = new Family();
    andersenFamily.setId("Andersen.1");
    andersenFamily.setLastName("Andersen");
    // More properties

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    this.client.createDocument(collectionLink, family, new RequestOptions(), true);
    ```

* <span data-ttu-id="a4c06-148">Provede se dotaz SQL přes JSON.</span><span class="sxs-lookup"><span data-stu-id="a4c06-148">A SQL query over JSON is performed.</span></span>

    ```java
    FeedOptions queryOptions = new FeedOptions();
    queryOptions.setPageSize(-1);
    queryOptions.setEnableCrossPartitionQuery(true);

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    FeedResponse<Document> queryResults = this.client.queryDocuments(
        collectionLink,
        "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", queryOptions);

    System.out.println("Running SQL query...");
    for (Document family : queryResults.getQueryIterable()) {
        System.out.println(String.format("\tRead %s", family));
    }
    ```    

## <a name="update-your-connection-string"></a><span data-ttu-id="a4c06-149">Aktualizace připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="a4c06-149">Update your connection string</span></span>

<span data-ttu-id="a4c06-150">Teď se vraťte zpátky na portál Azure Portal, kde najdete informace o připojovacím řetězci, a zkopírujte je do aplikace.</span><span class="sxs-lookup"><span data-stu-id="a4c06-150">Now go back to the Azure portal to get your connection string information and copy it into the app.</span></span> <span data-ttu-id="a4c06-151">Tím aplikaci umožníte komunikovat s hostovanou databází.</span><span class="sxs-lookup"><span data-stu-id="a4c06-151">This will enable your app to communicate with your hosted database.</span></span>

1. <span data-ttu-id="a4c06-152">Na webu [Azure Portal](http://portal.azure.com/) klikněte v účtu databáze Azure Cosmos v levém navigačním panelu na možnost **Klíče** a potom klikněte na **Klíče pro čtení i zápis**.</span><span class="sxs-lookup"><span data-stu-id="a4c06-152">In the [Azure portal](http://portal.azure.com/), in your Azure Cosmos DB account, in the left navigation click **Keys**, and then click **Read-write Keys**.</span></span> <span data-ttu-id="a4c06-153">V dalším kroku zkopírujete pomocí tlačítek kopírování na pravé straně obrazovky identifikátor URI a PRIMÁRNÍ KLÍČ do souboru `Program.java`.</span><span class="sxs-lookup"><span data-stu-id="a4c06-153">You'll use the copy buttons on the right side of the screen to copy the URI and PRIMARY KEY into the `Program.java` file in the next step.</span></span>

    ![Zobrazení a zkopírování přístupového klíče na webu Azure Portal v okně Klíče](./media/create-documentdb-dotnet/keys.png)

2. <span data-ttu-id="a4c06-155">V otevřeném souboru `Program.java` zkopírujte z portálu hodnotu identifikátoru URI (pomocí tlačítka kopírování) a nastavte ji jako hodnotu koncového bodu konstruktoru DocumentClient v souboru `Program.java`.</span><span class="sxs-lookup"><span data-stu-id="a4c06-155">In the open `Program.java` file, copy your URI value from the portal (using the copy button) and make it the value of the endpoint to the DocumentClient constructor in `Program.java`.</span></span> 

    `"https://FILLME.documents.azure.com"`

4. <span data-ttu-id="a4c06-156">Potom z portálu zkopírujte hodnotu PRIMÁRNÍHO KLÍČE a nahraďte jí parametr FILLME. Tím z ní uděláte druhý parametr v konstruktoru DocumentClient.</span><span class="sxs-lookup"><span data-stu-id="a4c06-156">Then copy your PRIMARY KEY value from the portal and paste it over “FILLME”, making it the second parameter in the DocumentClient constructor.</span></span> <span data-ttu-id="a4c06-157">Teď jste aktualizovali aplikaci a zadali do ní všechny informace potřebné ke komunikaci s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="a4c06-157">You've now updated your app with all the info it needs to communicate with Azure Cosmos DB.</span></span> 
    
## <a name="run-the-app"></a><span data-ttu-id="a4c06-158">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="a4c06-158">Run the app</span></span>

1. <span data-ttu-id="a4c06-159">V okně terminálu Git přejděte příkazem `cd` do složky azure-cosmos-db-documentdb-java-getting-started.</span><span class="sxs-lookup"><span data-stu-id="a4c06-159">In the git terminal window, `cd` to the azure-cosmos-db-documentdb-java-getting-started folder.</span></span>

2. <span data-ttu-id="a4c06-160">V okně terminálu Git zadejte `mvn package`, aby se nainstalovaly požadované balíčky Java.</span><span class="sxs-lookup"><span data-stu-id="a4c06-160">In the git terminal window, type `mvn package` to install the required Java packages.</span></span>

3. <span data-ttu-id="a4c06-161">V okně terminálu Git spuštěním příkazu `mvn exec:java -D exec.mainClass=GetStarted.Program` spusťte svoji aplikaci Java.</span><span class="sxs-lookup"><span data-stu-id="a4c06-161">In the git terminal window, run `mvn exec:java -D exec.mainClass=GetStarted.Program` in the terminal window to start your Java application.</span></span>

    <span data-ttu-id="a4c06-162">V okně terminálu se zobrazí oznámení o vytvoření databáze FamilyDB a výzva ke stisknutí klávesy a pokračování.</span><span class="sxs-lookup"><span data-stu-id="a4c06-162">In the terminal window, you'll receive notification that the FamilyDB database was created, and to press a key to continue.</span></span> <span data-ttu-id="a4c06-163">Stisknutím libovolné klávesy vytvořte databázi, potom přejděte do Průzkumníku dat a uvidíte, že teď obsahuje databázi FamilyDB.</span><span class="sxs-lookup"><span data-stu-id="a4c06-163">Press a key to create the database, then switch to the Data Explorer and you'll see that it now contains a FamilyDB database.</span></span> <span data-ttu-id="a4c06-164">Pokračujte a stiskem kláves vytvořte kolekci a dokumenty a potom proveďte dotaz.</span><span class="sxs-lookup"><span data-stu-id="a4c06-164">Continue to press keys to create the collection and the documents and then perform a query.</span></span> <span data-ttu-id="a4c06-165">Po dokončení projektu se prostředky odstraní z vašeho účtu.</span><span class="sxs-lookup"><span data-stu-id="a4c06-165">When the project completes, the resources are deleted from your account.</span></span> 

    ![Zobrazení a zkopírování přístupového klíče na webu Azure Portal v okně Klíče](./media/create-documentdb-java/console-output.png)


## <a name="review-slas-in-the-azure-portal"></a><span data-ttu-id="a4c06-167">Ověření smluv SLA na webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a4c06-167">Review SLAs in the Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="a4c06-168">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="a4c06-168">Clean up resources</span></span>

<span data-ttu-id="a4c06-169">Pokud nebudete tuto aplikace nadále používat, odstraňte na základě následujícího postupu z portálu Azure Portal všechny prostředky vytvořené podle tohoto rychlého startu:</span><span class="sxs-lookup"><span data-stu-id="a4c06-169">If you're not going to continue to use this app, delete all resources created by this quickstart in the Azure portal with the following steps:</span></span>

1. <span data-ttu-id="a4c06-170">V nabídce vlevo na portálu Azure Portal klikněte na **Skupiny prostředků** a pak klikněte na název vytvořeného prostředku.</span><span class="sxs-lookup"><span data-stu-id="a4c06-170">From the left-hand menu in the Azure portal, click **Resource groups** and then click the name of the resource you created.</span></span> 
2. <span data-ttu-id="a4c06-171">Na stránce skupiny prostředků klikněte na **Odstranit**, do textového pole zadejte prostředek, který chcete odstranit, a pak klikněte na **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="a4c06-171">On your resource group page, click **Delete**, type the name of the resource to delete in the text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a4c06-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a4c06-172">Next steps</span></span>

<span data-ttu-id="a4c06-173">V tomto rychlém startu jste se seznámili s postupem vytvoření účtu Azure Cosmos DB, databáze dokumentů a kolekce pomocí Průzkumníku dat a spuštění aplikace, která to samé udělá programově.</span><span class="sxs-lookup"><span data-stu-id="a4c06-173">In this quickstart, you've learned how to create an Azure Cosmos DB account, document database, and collection using the Data Explorer, and run an app to do the same thing programmatically.</span></span> <span data-ttu-id="a4c06-174">Teď můžete do účtu databáze Cosmos importovat další data.</span><span class="sxs-lookup"><span data-stu-id="a4c06-174">You can now import additional data to your Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="a4c06-175">Importování dat do služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="a4c06-175">Import data into Azure Cosmos DB</span></span>](import-data.md)


