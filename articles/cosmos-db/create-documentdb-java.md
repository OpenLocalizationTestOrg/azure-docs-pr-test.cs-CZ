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
# <a name="azure-cosmos-db-create-a-document-database-using-java-and-hello-azure-portal"></a>Azure Cosmos DB: Vytvoření dokumentu databáze pomocí Java a hello portálu Azure

Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku. Můžete rychle vytvořit a dotazovat dokumentu, klíč/hodnota a graf databází, které těžit z globální distribuční hello a možnosti vodorovné škálování jádrem hello Azure Cosmos DB. 

Tento rychlý start vytvoří dokument databázi pomocí hello nástroje Azure portálu pro Azure Cosmos DB. Tento rychlý start také ukazuje, jak vytvořit tooquickly konzolovou aplikaci Java pomocí hello [DocumentDB Java API](documentdb-sdk-java.md). Hello pokyny v tento rychlý start platí pro všechny operační systémy, které podporují Javu. Po dokončení tento rychlý start budete zkušenosti s vytvářením a úpravy dokumentu databázových prostředků v hello uživatelského rozhraní nebo programově, podle toho, co je vaši volbu.

## <a name="prerequisites"></a>Požadavky

* [Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
    * Ubuntu, spusťte `apt-get install default-jdk` tooinstall hello JDK.
    * Být jisti tooset hello JAVA_HOME prostředí proměnné toopoint toohello složka nainstalovanou hello JDK.
* [Stáhněte](http://maven.apache.org/download.cgi) a [nainstalujte](http://maven.apache.org/install.html) binární archiv [Maven](http://maven.apache.org/).
    * Na Ubuntu, můžete spustit `apt-get install maven` tooinstall Maven.
* [Git](https://www.git-scm.com/)
    * Na Ubuntu, můžete spustit `sudo apt-get install git` tooinstall Git.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Vytvoření účtu databáze

Než bude možné vytvořit databázi dokumentů, musíte toocreate účet databáze SQL (DocumentDB) s Azure Cosmos DB.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a>Přidání kolekce

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="add-sample-data"></a>
## <a name="add-sample-data"></a>Přidání ukázkových dat

Nyní můžete přidat tooyour nové shromažďování dat pomocí Průzkumníku dat.

1. V Průzkumníku dat hello nové databáze se zobrazí v podokně Kolekce hello. Rozbalte hello **úlohy** databáze, rozbalte položku hello **položky** kolekce, klikněte na tlačítko **dokumenty**a potom klikněte na **nové dokumenty**. 

   ![Vytváření nových dokumentů v Průzkumníku dat v hello portálu Azure](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-new-document.png)
  
2. Nyní přidejte toohello kolekce dokumentů s hello strukturu.

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries.",
         "isComplete": false
     }
     ```

3. Po přidání hello json toohello **dokumenty** , klikněte na **Uložit**.

    ![Kopírovat json data a klikněte na Uložit v Průzkumníku dat v hello portálu Azure](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-save-document.png)

4.  Vytvořte a uložte jeden další dokument, kde je vložit jedinečnou hodnotu pro hello `id` vlastnost a změňte hello ostatní vlastnosti podle potřeby. Nové dokumenty můžou mít jakoukoli strukturu, protože Azure Cosmos DB neuplatňuje pro data žádné schéma.

     Teď použijte dotazy v Průzkumníku dat tooretrieve vaše data kliknutím hello můžete **upravit filtr** a **použít filtr** tlačítka. Ve výchozím Průzkumníku dat používá `SELECT * FROM c` tooretrieve všechny dokumenty v hello kolekce, ale můžete změnit této tooa různých [dotazu SQL](documentdb-sql-query.md), jako například `SELECT * FROM c ORDER BY c._ts DESC`, tooreturn všechny dokumenty hello v sestupném pořadí podle jejich časové razítko. 
 
     Můžete také použít Průzkumníku dat toocreate uložené procedury, funkce UDF a aktivační události tooperform serverovou obchodní logiku také jako propustnost škálování. Průzkumník dat zpřístupní všechny hello předdefinované programový přístup k datům v hello rozhraní API k dispozici, ale poskytuje snadný přístup k datům tooyour v hello portálu Azure.

## <a name="clone-hello-sample-application"></a>Klonování hello ukázkové aplikace

Teď umožňuje přepnout tooworking s kódem. Pojďme klonovat aplikace DocumentDB API z Githubu, nastavte hello připojovací řetězec a spusťte ho. Uvidíte, jak je snadné toowork s daty prostřednictvím kódu programu. 

1. Otevřete okno terminálu git, jako je například git bash a `CD` tooa pracovní adresář.  

2. Spusťte následující příkaz tooclone hello Ukázka úložiště hello. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-java-getting-started.git
    ```

## <a name="review-hello-code"></a>Zkontrolujte hello kódu

Provedeme jejich stručný přehled o dění v aplikaci hello. Otevřete hello `Program.java` soubor ze složky \src\GetStarted hello a najděte tyto řádky kódu, které vytvořit hello prostředky Azure Cosmos DB. 

* Hello `DocumentClient` je inicializován.

    ```java
    this.client = new DocumentClient("https://FILLME.documents.azure.com",
            "FILLME", 
            new ConnectionPolicy(),
            ConsistencyLevel.Session);
    ```

* Vytvoří se nová databáze.

    ```java
    Database database = new Database();
    database.setId(databaseName);
    
    this.client.createDatabase(database, null);
    ```

* Vytvoří se nová kolekce.

    ```java
    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId(collectionName);

    ...

    this.client.createCollection(databaseLink, collectionInfo, requestOptions);
    ```

* Vytvoří se některé dokumenty.

    ```java
    // Any Java object within your code can be serialized into JSON and written tooAzure Cosmos DB
    Family andersenFamily = new Family();
    andersenFamily.setId("Andersen.1");
    andersenFamily.setLastName("Andersen");
    // More properties

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    this.client.createDocument(collectionLink, family, new RequestOptions(), true);
    ```

* Provede se dotaz SQL přes JSON.

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

## <a name="update-your-connection-string"></a>Aktualizace připojovacího řetězce

Nyní přejděte zpět toohello Azure portálu tooget vaše informace o připojovacím řetězci a zkopírujte jej do aplikace hello. Tato akce povolí toocommunicate vaší aplikace s hostované databáze.

1. V hello [portál Azure](http://portal.azure.com/), ve vašem Azure Cosmos DB účet, klikněte v levé navigační hello **klíče**a potom klikněte na **klíče pro čtení a zápis**. Použijete tlačítka hello kopírovat na pravé straně hello hello obrazovky toocopy hello URI a PRIMARY KEY do hello `Program.java` souboru v dalším kroku hello.

    ![Zobrazení a zkopírování přístupový klíč v hello portálu Azure, okna klíče](./media/create-documentdb-dotnet/keys.png)

2. V otevřené hello `Program.java` souboru, zkopírujte URI hodnota z portálu hello (pomocí tlačítka kopírování hello) a nastavit jej jako hello hodnotu hello koncový bod toohello DocumentClient konstruktor v `Program.java`. 

    `"https://FILLME.documents.azure.com"`

4. Poté zkopírujte primární klíč hodnota z portálu hello a vložte ho přes "FILLME", což hello druhý parametr v konstruktoru DocumentClient hello. Jste nyní aktualizovat vaši aplikaci s všechny údaje hello potřebuje toocommunicate s Azure Cosmos DB. 
    
## <a name="run-hello-app"></a>Spuštění aplikace hello

1. V okně terminálu hello git `cd` toohello azure-cosmos-db-documentdb-java-getting-started složky.

2. Zadejte v okně terminálu hello git, `mvn package` tooinstall hello požadované balíčky jazyka Java.

3. Okno terminálu hello git, spusťte `mvn exec:java -D exec.mainClass=GetStarted.Program` hello okno terminálu toostart aplikace v jazyce Java.

    V okně hello terminálu dostanete oznámení, že hello FamilyDB databáze byla vytvořena a toopress klíče toocontinue. Stiskněte klíče toocreate hello databáze, potom přepněte toohello Průzkumníku dat a uvidíte, že teď obsahuje FamilyDB databáze. I nadále toopress klíče toocreate hello kolekce a hello dokumenty a potom spustit dotaz. Po dokončení projektu hello hello prostředky budou odstraněny z vašeho účtu. 

    ![Zobrazení a zkopírování přístupový klíč v hello portálu Azure, okna klíče](./media/create-documentdb-java/console-output.png)


## <a name="review-slas-in-hello-azure-portal"></a>Zkontrolujte SLA v hello portálu Azure

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud ale nebudete toocontinue toouse této aplikace, odstraňte všechny prostředky, které jsou vytvořené tento rychlý start v hello portál Azure s hello následující kroky:

1. V levé nabídce hello v hello portálu Azure klikněte na **skupiny prostředků** a pak klikněte na název hello hello prostředků, které jste vytvořili. 
2. Na stránce skupiny prostředků, klikněte na tlačítko **odstranit**hello textového pole zadejte název hello toodelete hello prostředků a pak klikněte na tlačítko **odstranit**.

## <a name="next-steps"></a>Další kroky

V tento rychlý start když jste se naučili jak toocreate účet Azure Cosmos DB, databázi dokumentů a kolekce pomocí hello Průzkumníku dat a spuštění aplikace toodo hello samé prostřednictvím kódu programu. Nyní můžete importovat další data tooyour Cosmos DB účtu. 

> [!div class="nextstepaction"]
> [Importování dat do služby Azure Cosmos DB](import-data.md)


