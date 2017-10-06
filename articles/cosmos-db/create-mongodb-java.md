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
# <a name="azure-cosmos-db-build-a-mongodb-api-console-app-with-java-and-hello-azure-portal"></a>Azure Cosmos DB: Sestavení aplikace konzoly MongoDB rozhraní API s Java a hello portálu Azure

Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku. Můžete rychle vytvořit a dotazovat dokumentu, klíč/hodnota a graf databází, které těžit z globální distribuční hello a možnosti vodorovné škálování jádrem hello Azure Cosmos DB. 

Tento rychlý start předvádí, jak hello toocreate účet Azure Cosmos DB, dokumentu databáze a kolekce pomocí portálu Azure. Budete pak sestavení a nasazení aplikace konzoly založený na hello [MongoDB Java ovladač](https://docs.mongodb.com/ecosystem/drivers/java/). 

## <a name="prerequisites"></a>Požadavky

* Před spuštěním této ukázce, musíte mít hello následující požadavky:
   * JDK 1.7+ (pokud nemáte JDK, spusťte `apt-get install default-jdk`)
   * Maven (pokud nemáte Maven, spusťte `apt-get install maven`)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Vytvoření účtu databáze

[!INCLUDE [mongodb-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="add-a-collection"></a>Přidání kolekce

Novou databázi pojmenujte **db** a novou kolekci **coll**.

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="clone-hello-sample-application"></a>Klonování hello ukázkové aplikace

Teď umožňuje nastavit připojovací řetězec hello klonování MongoDB API aplikace z githubu a potom ho spusťte. Uvidíte, jak je snadné toowork s daty prostřednictvím kódu programu. 

1. Otevřete okno terminálu git, jako je například git bash a `cd` tooa pracovní adresář.  

2. Spusťte následující příkaz tooclone hello Ukázka úložiště hello. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-getting-started.git
    ```

3. Poté otevřete soubor řešení hello v sadě Visual Studio. 

## <a name="review-hello-code"></a>Zkontrolujte hello kódu

Provedeme jejich stručný přehled o dění v aplikaci hello. Otevřete hello `Program.cs` souboru a najdete, že tyto řádky kódu vytvořit hello prostředky Azure Cosmos DB. 

* Hello DocumentClient je inicializován.

    ```java
    MongoClientURI uri = new MongoClientURI("FILLME");`

    MongoClient mongoClient = new MongoClient(uri);            
    ```

* Vytvoří se nová databáze a kolekce.

    ```java
    MongoDatabase database = mongoClient.getDatabase("db");

    MongoCollection<Document> collection = database.getCollection("coll");
    ```

* Pomocí metody `MongoCollection.insertOne` se vloží některé dokumenty.

    ```java
    Document document = new Document("fruit", "apple")
    collection.insertOne(document);
    ```

* Pomocí metody `MongoCollection.find` se provedou některé dotazy.

    ```java
    Document queryResult = collection.find(Filters.eq("fruit", "apple")).first();
    System.out.println(queryResult.toJson());       
    ```

## <a name="update-your-connection-string"></a>Aktualizace připojovacího řetězce

Nyní přejděte zpět toohello Azure portálu tooget vaše informace o připojovacím řetězci a zkopírujte jej do aplikace hello.

1. Hello účtu, vyberte **rychlý Start**vyberte Java a pak zkopírujte hello připojovací řetězec tooyour schránky

2. Otevřete hello `Program.java` souboru, nahraďte hello argument toohello MongoClientURI konstruktor hello připojovací řetězec. Jste nyní aktualizovat vaši aplikaci s všechny údaje hello potřebuje toocommunicate s Azure Cosmos DB. 
    
## <a name="run-hello-console-app"></a>Spusťte konzolovou aplikaci hello

1. Spustit `mvn package` v terminálu tooinstall požadované moduly npm

2. Spustit `mvn exec:java -D exec.mainClass=GetStarted.Program` v terminálu toostart aplikace v jazyce Java.

Teď můžete použít [Robomongo](mongodb-robomongo.md) / [Studio 3T](mongodb-mongochef.md) tooquery, upravit a pracovat s Tato nová data.

## <a name="review-slas-in-hello-azure-portal"></a>Zkontrolujte SLA v hello portálu Azure

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud ale nebudete toocontinue toouse této aplikace, odstraňte všechny prostředky, které jsou vytvořené tento rychlý start v hello portál Azure s hello následující kroky:

1. V levé nabídce hello v hello portálu Azure klikněte na **skupiny prostředků** a pak klikněte na název hello hello prostředků, které jste vytvořili. 
2. Na stránce skupiny prostředků, klikněte na tlačítko **odstranit**hello textového pole zadejte název hello toodelete hello prostředků a pak klikněte na tlačítko **odstranit**.

## <a name="next-steps"></a>Další kroky

V tento rychlý start když jste se naučili jak toocreate účtu Azure Cosmos DB, vytvořte kolekci pomocí hello Průzkumníku dat a spusťte konzolovou aplikaci. Nyní můžete importovat další data tooyour Cosmos DB účtu. 

> [!div class="nextstepaction"]
> [Importování dat MongoDB do služby Azure Cosmos DB](mongodb-migrate.md)


