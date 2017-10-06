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
# <a name="nosql-tutorial-build-a-documentdb-api-java-console-application"></a>Kurzu NoSQL: vytvoření konzolové aplikace DocumentDB Java rozhraní API
> [!div class="op_single_selector"]
> * [.NET](documentdb-get-started.md)
> * [.NET Core](documentdb-dotnetcore-get-started.md)
> * [Node.js pro MongoDB](mongodb-samples.md)
> * [Node.js](documentdb-nodejs-get-started.md)
> * [Java](documentdb-java-get-started.md)
> * [C++](documentdb-cpp-get-started.md)
>  
> 

Vítejte v kurzu NoSQL toohello pro hello DocumentDB rozhraní API pro Azure Cosmos DB Java SDK! Až projdete tímto kurzem, budete mít konzolovou aplikaci, která vytváří prostředky Azure Cosmos DB a dotazuje se na ně.

Kurz zahrnuje:

* Vytvoření a připojení účtu Azure Cosmos DB tooan
* Konfigurace řešení v nástroji Visual Studio
* Vytvoření online databáze
* Vytvoření kolekce
* Vytvoření dokumentů JSON
* Dotazování na kolekci hello
* Vytvoření dokumentů JSON
* Dotazování na kolekci hello
* Nahrazení dokumentu
* Odstranění dokumentu
* Odstraňování databáze aplikace hello

Můžeme začít!

## <a name="prerequisites"></a>Požadavky
Ujistěte se, že máte hello následující:

* Aktivní účet Azure. Pokud žádný nemáte, můžete si zaregistrovat [bezplatný účet](https://azure.microsoft.com/free/). Alternativně můžete použít hello [emulátoru DB Cosmos Azure](local-emulator.md) pro účely tohoto kurzu.
* [Git](https://git-scm.com/downloads)
* [Java Development Kit (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
* [Maven](http://maven.apache.org/download.cgi).

## <a name="step-1-create-an-azure-cosmos-db-account"></a>Krok 1: Vytvoření účtu služby Azure Cosmos DB
Vytvořme účet služby Azure Cosmos DB. Pokud již máte účet, který chcete toouse, můžete přeskočit příliš[klon hello Githubu projektu](#GitClone). Pokud používáte hello emulátoru DB Cosmos Azure, postupujte podle kroků hello v [emulátoru DB Cosmos Azure](local-emulator.md) tooset až hello emulátoru a přeskočit příliš[klon hello Githubu projektu](#GitClone).

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="GitClone"></a>Krok 2: Klonování hello Githubu projektu
Abyste mohli začít klonováním hello úložiště GitHub pro [Začínáme s Azure Cosmos DB a Java](https://github.com/Azure-Samples/documentdb-java-getting-started). Například z místního adresáře spusťte hello následující tooretrieve hello ukázkový projekt místně.

    git clone git@github.com:Azure-Samples/azure-cosmos-db-documentdb-java-getting-started.git

    cd azure-cosmos-db-documentdb-java-getting-started

adresář Hello obsahuje `pom.xml` pro projekt hello a `src` složku obsahující Java zdrojového kódu včetně `Program.java` které ukazuje, jak provádějí jednoduché operace s Azure DB Cosmos jako je vytváření dokumentů a dotazování na data v rámci kolekce. Hello `pom.xml` obsahuje závislost na hello [DocumentDB Java SDK na Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb).

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-documentdb</artifactId>
        <version>LATEST</version>
    </dependency>

## <a id="Connect"></a>Krok 3: Připojení účtu Azure Cosmos DB tooan
V dalším kroku head zpět toohello [portálu Azure](https://portal.azure.com) tooretrieve koncový bod a primární hlavní klíč. Hello koncový bod Azure Cosmos DB a primární klíč jsou nezbytné, aby vaše aplikace toounderstand kde tooconnect a u Azure Cosmos DB tootrust připojení vaší aplikace.

V hello portálu Azure, přejděte tooyour Azure Cosmos DB účtu a pak klikněte na tlačítko **klíče**. Zkopírujte z portálu hello hello URI a vložte ji do `https://FILLME.documents.azure.com` v souboru Program.java hello. Kopírování hello primární klíč z portálu hello a vložte ji do `FILLME`.

    this.client = new DocumentClient(
        "https://FILLME.documents.azure.com",
        "FILLME"
        , new ConnectionPolicy(),
        ConsistencyLevel.Session);

![Snímek obrazovky hello používá hello toocreate kurzu NoSQL konzolovou aplikaci Java portálu Azure. Zobrazuje Azure DB Cosmos účet s AKTIVNÍM centrem hello zvýrazní, hello tlačítkem klíče v okně účtu Azure Cosmos DB hello a hodnotami URI, primární klíč a sekundární klíč hello zvýrazněným hello okna klíče][keys]

## <a name="step-4-create-a-database"></a>Krok 4: Vytvoření databáze
Vaše Azure DB Cosmos [databáze](documentdb-resources.md#databases) lze vytvořit pomocí hello [metodu createDatabase](/java/api/com.microsoft.azure.documentdb._document_client.createdatabase) metoda hello **DocumentClient** třídy. Databáze je logický kontejner úložiště dokumentů JSON rozděleného mezi kolekcemi hello.

    Database database = new Database();
    database.setId("familydb");
    this.client.createDatabase(database, null);

## <a id="CreateColl"></a>Krok 5: Vytvoření kolekce
> [!WARNING]
> **createCollection** vytvoří novou kolekci s vyhrazenou propustností, za kterou se hradí poplatky. Další podrobnosti najdete na [stránce s cenami](https://azure.microsoft.com/pricing/details/cosmos-db/).
> 
> 

A [kolekce](documentdb-resources.md#collections) lze vytvořit pomocí hello [createCollection](/java/api/com.microsoft.azure.documentdb._document_client.createcollection) metoda hello **DocumentClient** třídy. Kolekce je kontejner dokumentů JSON a přidružené logiky javascriptové aplikace.


    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId("familycoll");

    // Azure Cosmos DB collections can be reserved with throughput specified in request units/second. 
    // Here we create a collection with 400 RU/s.
    RequestOptions requestOptions = new RequestOptions();
    requestOptions.setOfferThroughput(400);

    this.client.createCollection("/dbs/familydb", collectionInfo, requestOptions);

## <a id="CreateDoc"></a>Krok 6: Vytvoření dokumentů JSON
A [dokumentu](documentdb-resources.md#documents) lze vytvořit pomocí hello [createDocument](/java/api/com.microsoft.azure.documentdb._document_client.createdocument) metoda hello **DocumentClient** třídy. Dokumenty představují uživatelem definovaný (libovolný) obsah JSON. Nyní můžete vložit jeden nebo více dokumentů. Pokud již máte data, které byste chtěli toostore v databázi, můžete použít Azure Cosmos DB [nástroj pro migraci dat](import-data.md) tooimport hello data do databáze.

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

## <a id="Query"></a>Krok 7: Dotazování prostředků Azure Cosmos DB
Azure Cosmos DB podporuje bohaté [dotazy](documentdb-sql-query.md) na dokumenty JSON uložené v každé z kolekcí.  Hello následující vzorový kód ukazuje, jak tooquery dokumentů v Azure Cosmos DB pomocí syntaxe jazyka SQL s hello [dokumenty dotazu](/java/api/com.microsoft.azure.documentdb._document_client.querydocuments) metoda.

    FeedResponse<Document> queryResults = this.client.queryDocuments(
        "/dbs/familydb/colls/familycoll",
        "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", 
        null);

    System.out.println("Running SQL query...");
    for (Document family : queryResults.getQueryIterable()) {
        System.out.println(String.format("\tRead %s", family));
    }

## <a id="ReplaceDocument"></a>Krok 8: Nahrazení dokumentu JSON
Aktualizace dokumentů JSON pomocí hello podporuje Azure Cosmos DB [replaceDocument](/java/api/com.microsoft.azure.documentdb._document_client.replacedocument) metoda.

    // Update a property
    andersenFamily.Children[0].Grade = 6;

    this.client.replaceDocument(
        "/dbs/familydb/colls/familycoll/docs/Andersen.1", 
        andersenFamily,
        null);

## <a id="DeleteDocument"></a>Krok 9: Odstranění dokumentu JSON
Podobně Azure Cosmos DB podporuje odstraňování dokumentů JSON pomocí hello [deleteDocument](/java/api/com.microsoft.azure.documentdb._document_client.deletedocument) metoda.  

    this.client.delete("/dbs/familydb/colls/familycoll/docs/Andersen.1", null);

## <a id="DeleteDatabase"></a>Krok 10: Odstranění databáze hello
Odstraňování databáze hello vytvořili odebere hello databáze a všech jejích podřízených prostředků (kolekcí, dokumentů atd.).

    this.client.deleteDatabase("/dbs/familydb", null);

## <a id="Run"></a>Krok 11: Spuštění celé konzolové aplikace jazyka Java!
toorun hello aplikace z konzoly hello přejděte složky toohello projektu a kompilace pomocí nástroje Maven:
    
    mvn package

Spuštění `mvn package` stáhne nejnovější knihovny Azure Cosmos DB hello z Maven a vytváří `GetStarted-0.0.1-SNAPSHOT.jar`. Spusťte aplikaci hello spuštěním:

    mvn exec:java -D exec.mainClass=GetStarted.Program

Blahopřejeme! Dokončili jste tento kurz NoSQL a máte funkční konzolovou aplikaci jazyka Java!

## <a name="next-steps"></a>Další kroky
* Chcete kurz vývoje webové aplikace v jazyce Java? Viz [Vytvoření webové aplikace pomocí Javy a služby Azure Cosmos DB](documentdb-java-application.md).
* Zjistěte, jak příliš[monitorovat účet Azure Cosmos DB](monitor-accounts.md).
* Spouštění dotazů na našem ukázkovou datovou sadu v hello [Query Playground](https://www.documentdb.com/sql/demo).
* Další informace o programovacím modelu hello hello vývoj části hello [stránce dokumentace Azure Cosmos DB](https://azure.microsoft.com/documentation/services/documentdb/).

[keys]: media/documentdb-get-started/nosql-tutorial-keys.png
