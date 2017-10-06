---
title: "aaaCreate databázi Azure Cosmos DB dokument s Javou | Microsoft Docs | Microsoft Docs"
description: "Uvede ukázku kódu Java pomocí dotazu tooand tooconnect hello rozhraní API služby Azure Cosmos databáze DocumentDB"
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
ms.openlocfilehash: 400c9e7780034d3e28d749e734786e950edad22f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-create-a-document-database-using-java-and-hello-azure-portal"></a><span data-ttu-id="dc26c-103">Azure Cosmos DB: Vytvoření dokumentu databáze pomocí Java a hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="dc26c-103">Azure Cosmos DB: Create a document database using Java and hello Azure portal</span></span>

<span data-ttu-id="dc26c-104">Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku.</span><span class="sxs-lookup"><span data-stu-id="dc26c-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="dc26c-105">Můžete rychle vytvořit a dotazovat dokumentu, klíč/hodnota a graf databází, které těžit z globální distribuční hello a možnosti vodorovné škálování jádrem hello Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="dc26c-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="dc26c-106">Tento rychlý start vytvoří dokument databázi pomocí hello nástroje Azure portálu pro Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="dc26c-106">This quickstart creates a document database using hello Azure portal tools for Azure Cosmos DB.</span></span> <span data-ttu-id="dc26c-107">Tento rychlý start také ukazuje, jak vytvořit tooquickly konzolovou aplikaci Java pomocí hello [DocumentDB Java API](documentdb-sdk-java.md).</span><span class="sxs-lookup"><span data-stu-id="dc26c-107">This quickstart also shows you how tooquickly create a Java console app using hello [DocumentDB Java API](documentdb-sdk-java.md).</span></span> <span data-ttu-id="dc26c-108">Hello pokyny v tento rychlý start platí pro všechny operační systémy, které podporují Javu.</span><span class="sxs-lookup"><span data-stu-id="dc26c-108">hello instructions in this quickstart can be followed on any operating system that is capable of running Java.</span></span> <span data-ttu-id="dc26c-109">Po dokončení tento rychlý start budete zkušenosti s vytvářením a úpravy dokumentu databázových prostředků v hello uživatelského rozhraní nebo programově, podle toho, co je vaši volbu.</span><span class="sxs-lookup"><span data-stu-id="dc26c-109">By completing this quickstart you'll be familiar with creating and modifying document database resources in either hello UI or programatically, whichever is your preference.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dc26c-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="dc26c-110">Prerequisites</span></span>

* [<span data-ttu-id="dc26c-111">Java Development Kit (JDK) 1.7+</span><span class="sxs-lookup"><span data-stu-id="dc26c-111">Java Development Kit (JDK) 1.7+</span></span>](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
    * <span data-ttu-id="dc26c-112">Ubuntu, spusťte `apt-get install default-jdk` tooinstall hello JDK.</span><span class="sxs-lookup"><span data-stu-id="dc26c-112">On Ubuntu, run `apt-get install default-jdk` tooinstall hello JDK.</span></span>
    * <span data-ttu-id="dc26c-113">Být jisti tooset hello JAVA_HOME prostředí proměnné toopoint toohello složka nainstalovanou hello JDK.</span><span class="sxs-lookup"><span data-stu-id="dc26c-113">Be sure tooset hello JAVA_HOME environment variable toopoint toohello folder where hello JDK is installed.</span></span>
* <span data-ttu-id="dc26c-114">[Stáhněte](http://maven.apache.org/download.cgi) a [nainstalujte](http://maven.apache.org/install.html) binární archiv [Maven](http://maven.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="dc26c-114">[Download](http://maven.apache.org/download.cgi) and [install](http://maven.apache.org/install.html) a [Maven](http://maven.apache.org/) binary archive</span></span>
    * <span data-ttu-id="dc26c-115">Na Ubuntu, můžete spustit `apt-get install maven` tooinstall Maven.</span><span class="sxs-lookup"><span data-stu-id="dc26c-115">On Ubuntu, you can run `apt-get install maven` tooinstall Maven.</span></span>
* [<span data-ttu-id="dc26c-116">Git</span><span class="sxs-lookup"><span data-stu-id="dc26c-116">Git</span></span>](https://www.git-scm.com/)
    * <span data-ttu-id="dc26c-117">Na Ubuntu, můžete spustit `sudo apt-get install git` tooinstall Git.</span><span class="sxs-lookup"><span data-stu-id="dc26c-117">On Ubuntu, you can run `sudo apt-get install git` tooinstall Git.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="dc26c-118">Vytvoření účtu databáze</span><span class="sxs-lookup"><span data-stu-id="dc26c-118">Create a database account</span></span>

<span data-ttu-id="dc26c-119">Než bude možné vytvořit databázi dokumentů, musíte toocreate účet databáze SQL (DocumentDB) s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="dc26c-119">Before you can create a document database, you need toocreate a SQL (DocumentDB) database account with Azure Cosmos DB.</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a><span data-ttu-id="dc26c-120">Přidání kolekce</span><span class="sxs-lookup"><span data-stu-id="dc26c-120">Add a collection</span></span>

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="add-sample-data"></a>
## <a name="add-sample-data"></a><span data-ttu-id="dc26c-121">Přidání ukázkových dat</span><span class="sxs-lookup"><span data-stu-id="dc26c-121">Add sample data</span></span>

<span data-ttu-id="dc26c-122">Nyní můžete přidat tooyour nové shromažďování dat pomocí Průzkumníku dat.</span><span class="sxs-lookup"><span data-stu-id="dc26c-122">You can now add data tooyour new collection using Data Explorer.</span></span>

1. <span data-ttu-id="dc26c-123">V Průzkumníku dat hello nové databáze se zobrazí v podokně Kolekce hello.</span><span class="sxs-lookup"><span data-stu-id="dc26c-123">In Data Explorer, hello new database appears in hello Collections pane.</span></span> <span data-ttu-id="dc26c-124">Rozbalte hello **úlohy** databáze, rozbalte položku hello **položky** kolekce, klikněte na tlačítko **dokumenty**a potom klikněte na **nové dokumenty**.</span><span class="sxs-lookup"><span data-stu-id="dc26c-124">Expand hello **Tasks** database, expand hello **Items** collection, click **Documents**, and then click **New Documents**.</span></span> 

   ![Vytváření nových dokumentů v Průzkumníku dat v hello portálu Azure](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-new-document.png)
  
2. <span data-ttu-id="dc26c-126">Nyní přidejte toohello kolekce dokumentů s hello strukturu.</span><span class="sxs-lookup"><span data-stu-id="dc26c-126">Now add a document toohello collection with hello following structure.</span></span>

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries.",
         "isComplete": false
     }
     ```

3. <span data-ttu-id="dc26c-127">Po přidání hello json toohello **dokumenty** , klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="dc26c-127">Once you've added hello json toohello **Documents** tab, click **Save**.</span></span>

    ![Kopírovat json data a klikněte na Uložit v Průzkumníku dat v hello portálu Azure](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-save-document.png)

4.  <span data-ttu-id="dc26c-129">Vytvořte a uložte jeden další dokument, kde je vložit jedinečnou hodnotu pro hello `id` vlastnost a změňte hello ostatní vlastnosti podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="dc26c-129">Create and save one more document where you insert a unique value for hello `id` property, and change hello other properties as you see fit.</span></span> <span data-ttu-id="dc26c-130">Nové dokumenty můžou mít jakoukoli strukturu, protože Azure Cosmos DB neuplatňuje pro data žádné schéma.</span><span class="sxs-lookup"><span data-stu-id="dc26c-130">Your new documents can have any structure you want as Azure Cosmos DB doesn't impose any schema on your data.</span></span>

     <span data-ttu-id="dc26c-131">Teď použijte dotazy v Průzkumníku dat tooretrieve vaše data kliknutím hello můžete **upravit filtr** a **použít filtr** tlačítka.</span><span class="sxs-lookup"><span data-stu-id="dc26c-131">You can now use queries in Data Explorer tooretrieve your data by clicking hello **Edit Filter** and **Apply Filter** buttons.</span></span> <span data-ttu-id="dc26c-132">Ve výchozím Průzkumníku dat používá `SELECT * FROM c` tooretrieve všechny dokumenty v hello kolekce, ale můžete změnit této tooa různých [dotazu SQL](documentdb-sql-query.md), jako například `SELECT * FROM c ORDER BY c._ts DESC`, tooreturn všechny dokumenty hello v sestupném pořadí podle jejich časové razítko.</span><span class="sxs-lookup"><span data-stu-id="dc26c-132">By default, Data Explorer uses `SELECT * FROM c` tooretrieve all documents in hello collection, but you can change that tooa different [SQL query](documentdb-sql-query.md), such as `SELECT * FROM c ORDER BY c._ts DESC`, tooreturn all hello documents in descending order based on their timestamp.</span></span> 
 
     <span data-ttu-id="dc26c-133">Můžete také použít Průzkumníku dat toocreate uložené procedury, funkce UDF a aktivační události tooperform serverovou obchodní logiku také jako propustnost škálování.</span><span class="sxs-lookup"><span data-stu-id="dc26c-133">You can also use Data Explorer toocreate stored procedures, UDFs, and triggers tooperform server-side business logic as well as scale throughput.</span></span> <span data-ttu-id="dc26c-134">Průzkumník dat zpřístupní všechny hello předdefinované programový přístup k datům v hello rozhraní API k dispozici, ale poskytuje snadný přístup k datům tooyour v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="dc26c-134">Data Explorer exposes all of hello built-in programmatic data access available in hello APIs, but provides easy access tooyour data in hello Azure portal.</span></span>

## <a name="clone-hello-sample-application"></a><span data-ttu-id="dc26c-135">Klonování hello ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="dc26c-135">Clone hello sample application</span></span>

<span data-ttu-id="dc26c-136">Teď umožňuje přepnout tooworking s kódem.</span><span class="sxs-lookup"><span data-stu-id="dc26c-136">Now let's switch tooworking with code.</span></span> <span data-ttu-id="dc26c-137">Pojďme klonovat aplikace DocumentDB API z Githubu, nastavte hello připojovací řetězec a spusťte ho.</span><span class="sxs-lookup"><span data-stu-id="dc26c-137">Let's clone a DocumentDB API app from GitHub, set hello connection string, and run it.</span></span> <span data-ttu-id="dc26c-138">Uvidíte, jak je snadné toowork s daty prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="dc26c-138">You see how easy it is toowork with data programmatically.</span></span> 

1. <span data-ttu-id="dc26c-139">Otevřete okno terminálu git, jako je například git bash a `CD` tooa pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="dc26c-139">Open a git terminal window, such as git bash, and `CD` tooa working directory.</span></span>  

2. <span data-ttu-id="dc26c-140">Spusťte následující příkaz tooclone hello Ukázka úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="dc26c-140">Run hello following command tooclone hello sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-java-getting-started.git
    ```

## <a name="review-hello-code"></a><span data-ttu-id="dc26c-141">Zkontrolujte hello kódu</span><span class="sxs-lookup"><span data-stu-id="dc26c-141">Review hello code</span></span>

<span data-ttu-id="dc26c-142">Provedeme jejich stručný přehled o dění v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="dc26c-142">Let's make a quick review of what's happening in hello app.</span></span> <span data-ttu-id="dc26c-143">Otevřete hello `Program.java` soubor ze složky \src\GetStarted hello a najděte tyto řádky kódu, které vytvořit hello prostředky Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="dc26c-143">Open hello `Program.java` file from hello \src\GetStarted folder, and find these lines of code that create hello Azure Cosmos DB resources.</span></span> 

* <span data-ttu-id="dc26c-144">Hello `DocumentClient` je inicializován.</span><span class="sxs-lookup"><span data-stu-id="dc26c-144">hello `DocumentClient` is initialized.</span></span>

    ```java
    this.client = new DocumentClient("https://FILLME.documents.azure.com",
            "FILLME", 
            new ConnectionPolicy(),
            ConsistencyLevel.Session);
    ```

* <span data-ttu-id="dc26c-145">Vytvoří se nová databáze.</span><span class="sxs-lookup"><span data-stu-id="dc26c-145">A new database is created.</span></span>

    ```java
    Database database = new Database();
    database.setId(databaseName);
    
    this.client.createDatabase(database, null);
    ```

* <span data-ttu-id="dc26c-146">Vytvoří se nová kolekce.</span><span class="sxs-lookup"><span data-stu-id="dc26c-146">A new collection is created.</span></span>

    ```java
    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId(collectionName);

    ...

    this.client.createCollection(databaseLink, collectionInfo, requestOptions);
    ```

* <span data-ttu-id="dc26c-147">Vytvoří se některé dokumenty.</span><span class="sxs-lookup"><span data-stu-id="dc26c-147">Some documents are created.</span></span>

    ```java
    // Any Java object within your code can be serialized into JSON and written tooAzure Cosmos DB
    Family andersenFamily = new Family();
    andersenFamily.setId("Andersen.1");
    andersenFamily.setLastName("Andersen");
    // More properties

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    this.client.createDocument(collectionLink, family, new RequestOptions(), true);
    ```

* <span data-ttu-id="dc26c-148">Provede se dotaz SQL přes JSON.</span><span class="sxs-lookup"><span data-stu-id="dc26c-148">A SQL query over JSON is performed.</span></span>

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

## <a name="update-your-connection-string"></a><span data-ttu-id="dc26c-149">Aktualizace připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="dc26c-149">Update your connection string</span></span>

<span data-ttu-id="dc26c-150">Nyní přejděte zpět toohello Azure portálu tooget vaše informace o připojovacím řetězci a zkopírujte jej do aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="dc26c-150">Now go back toohello Azure portal tooget your connection string information and copy it into hello app.</span></span> <span data-ttu-id="dc26c-151">Tato akce povolí toocommunicate vaší aplikace s hostované databáze.</span><span class="sxs-lookup"><span data-stu-id="dc26c-151">This will enable your app toocommunicate with your hosted database.</span></span>

1. <span data-ttu-id="dc26c-152">V hello [portál Azure](http://portal.azure.com/), ve vašem Azure Cosmos DB účet, klikněte v levé navigační hello **klíče**a potom klikněte na **klíče pro čtení a zápis**.</span><span class="sxs-lookup"><span data-stu-id="dc26c-152">In hello [Azure portal](http://portal.azure.com/), in your Azure Cosmos DB account, in hello left navigation click **Keys**, and then click **Read-write Keys**.</span></span> <span data-ttu-id="dc26c-153">Použijete tlačítka hello kopírovat na pravé straně hello hello obrazovky toocopy hello URI a PRIMARY KEY do hello `Program.java` souboru v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="dc26c-153">You'll use hello copy buttons on hello right side of hello screen toocopy hello URI and PRIMARY KEY into hello `Program.java` file in hello next step.</span></span>

    ![Zobrazení a zkopírování přístupový klíč v hello portálu Azure, okna klíče](./media/create-documentdb-dotnet/keys.png)

2. <span data-ttu-id="dc26c-155">V otevřené hello `Program.java` souboru, zkopírujte URI hodnota z portálu hello (pomocí tlačítka kopírování hello) a nastavit jej jako hello hodnotu hello koncový bod toohello DocumentClient konstruktor v `Program.java`.</span><span class="sxs-lookup"><span data-stu-id="dc26c-155">In hello open `Program.java` file, copy your URI value from hello portal (using hello copy button) and make it hello value of hello endpoint toohello DocumentClient constructor in `Program.java`.</span></span> 

    `"https://FILLME.documents.azure.com"`

4. <span data-ttu-id="dc26c-156">Poté zkopírujte primární klíč hodnota z portálu hello a vložte ho přes "FILLME", což hello druhý parametr v konstruktoru DocumentClient hello.</span><span class="sxs-lookup"><span data-stu-id="dc26c-156">Then copy your PRIMARY KEY value from hello portal and paste it over “FILLME”, making it hello second parameter in hello DocumentClient constructor.</span></span> <span data-ttu-id="dc26c-157">Jste nyní aktualizovat vaši aplikaci s všechny údaje hello potřebuje toocommunicate s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="dc26c-157">You've now updated your app with all hello info it needs toocommunicate with Azure Cosmos DB.</span></span> 
    
## <a name="run-hello-app"></a><span data-ttu-id="dc26c-158">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="dc26c-158">Run hello app</span></span>

1. <span data-ttu-id="dc26c-159">V okně terminálu hello git `cd` toohello azure-cosmos-db-documentdb-java-getting-started složky.</span><span class="sxs-lookup"><span data-stu-id="dc26c-159">In hello git terminal window, `cd` toohello azure-cosmos-db-documentdb-java-getting-started folder.</span></span>

2. <span data-ttu-id="dc26c-160">Zadejte v okně terminálu hello git, `mvn package` tooinstall hello požadované balíčky jazyka Java.</span><span class="sxs-lookup"><span data-stu-id="dc26c-160">In hello git terminal window, type `mvn package` tooinstall hello required Java packages.</span></span>

3. <span data-ttu-id="dc26c-161">Okno terminálu hello git, spusťte `mvn exec:java -D exec.mainClass=GetStarted.Program` hello okno terminálu toostart aplikace v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="dc26c-161">In hello git terminal window, run `mvn exec:java -D exec.mainClass=GetStarted.Program` in hello terminal window toostart your Java application.</span></span>

    <span data-ttu-id="dc26c-162">V okně hello terminálu dostanete oznámení, že hello FamilyDB databáze byla vytvořena a toopress klíče toocontinue.</span><span class="sxs-lookup"><span data-stu-id="dc26c-162">In hello terminal window, you'll receive notification that hello FamilyDB database was created, and toopress a key toocontinue.</span></span> <span data-ttu-id="dc26c-163">Stiskněte klíče toocreate hello databáze, potom přepněte toohello Průzkumníku dat a uvidíte, že teď obsahuje FamilyDB databáze.</span><span class="sxs-lookup"><span data-stu-id="dc26c-163">Press a key toocreate hello database, then switch toohello Data Explorer and you'll see that it now contains a FamilyDB database.</span></span> <span data-ttu-id="dc26c-164">I nadále toopress klíče toocreate hello kolekce a hello dokumenty a potom spustit dotaz.</span><span class="sxs-lookup"><span data-stu-id="dc26c-164">Continue toopress keys toocreate hello collection and hello documents and then perform a query.</span></span> <span data-ttu-id="dc26c-165">Po dokončení projektu hello hello prostředky budou odstraněny z vašeho účtu.</span><span class="sxs-lookup"><span data-stu-id="dc26c-165">When hello project completes, hello resources are deleted from your account.</span></span> 

    ![Zobrazení a zkopírování přístupový klíč v hello portálu Azure, okna klíče](./media/create-documentdb-java/console-output.png)


## <a name="review-slas-in-hello-azure-portal"></a><span data-ttu-id="dc26c-167">Zkontrolujte SLA v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="dc26c-167">Review SLAs in hello Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="dc26c-168">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="dc26c-168">Clean up resources</span></span>

<span data-ttu-id="dc26c-169">Pokud ale nebudete toocontinue toouse této aplikace, odstraňte všechny prostředky, které jsou vytvořené tento rychlý start v hello portál Azure s hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="dc26c-169">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span>

1. <span data-ttu-id="dc26c-170">V levé nabídce hello v hello portálu Azure klikněte na **skupiny prostředků** a pak klikněte na název hello hello prostředků, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="dc26c-170">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="dc26c-171">Na stránce skupiny prostředků, klikněte na tlačítko **odstranit**hello textového pole zadejte název hello toodelete hello prostředků a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="dc26c-171">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dc26c-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dc26c-172">Next steps</span></span>

<span data-ttu-id="dc26c-173">V tento rychlý start když jste se naučili jak toocreate účet Azure Cosmos DB, databázi dokumentů a kolekce pomocí hello Průzkumníku dat a spuštění aplikace toodo hello samé prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="dc26c-173">In this quickstart, you've learned how toocreate an Azure Cosmos DB account, document database, and collection using hello Data Explorer, and run an app toodo hello same thing programmatically.</span></span> <span data-ttu-id="dc26c-174">Nyní můžete importovat další data tooyour Cosmos DB účtu.</span><span class="sxs-lookup"><span data-stu-id="dc26c-174">You can now import additional data tooyour Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="dc26c-175">Importování dat do služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="dc26c-175">Import data into Azure Cosmos DB</span></span>](import-data.md)


