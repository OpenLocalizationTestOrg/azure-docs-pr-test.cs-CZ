---
title: "Azure Cosmos DB: Sestavení aplikace pomocí Node.js a hello rozhraní API DocumentDB | Microsoft Docs"
description: "Uvede ukázku kódu Node.js pomocí dotazu tooand tooconnect hello rozhraní API služby Azure Cosmos databáze DocumentDB"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 9c0f033c-240e-4fee-8421-08907231087f
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: 287d860c7d6f788f05a397b238ef0f841c3c30ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-documentdb-api-app-with-nodejs-and-hello-azure-portal"></a>Azure Cosmos DB: Sestavení aplikace DocumentDB rozhraní API pomocí Node.js a hello portálu Azure

Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku. Můžete rychle vytvořit a dotazovat dokumentu, klíč/hodnota a graf databází, které těžit z globální distribuční hello a možnosti vodorovné škálování jádrem hello Azure Cosmos DB. 

Tento rychlý start předvádí, jak hello toocreate účet Azure Cosmos DB, dokumentu databáze a kolekce pomocí portálu Azure. Potom sestavení a spuštění konzoly aplikace založená na hello [DocumentDB Node.js API](documentdb-sdk-node.md).

## <a name="prerequisites"></a>Požadavky

* Před spuštěním této ukázce, musíte mít hello následující požadavky:
    * [Node.js](https://nodejs.org/en/) verze 0.10.29 nebo vyšší
    * [Git](http://git-scm.com/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Vytvoření účtu databáze

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a>Přidání kolekce

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="clone-hello-sample-application"></a>Klonování hello ukázkové aplikace

Teď umožňuje nastavit připojovací řetězec hello klonování DocumentDB API aplikace z githubu a potom ho spusťte. Uvidíte, jak je snadné toowork s daty prostřednictvím kódu programu. 

1. Otevřete okno terminálu git, jako je například git bash a `CD` tooa pracovní adresář.  

2. Spusťte následující příkaz tooclone hello Ukázka úložiště hello. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-nodejs-getting-started.git
    ```

## <a name="review-hello-code"></a>Zkontrolujte hello kódu

Provedeme jejich stručný přehled o dění v aplikaci hello. Otevřete hello `app.js` souboru a najít, že tyto řádky kódu vytvořit hello prostředky Azure Cosmos DB. 

* Hello `documentClient` je inicializován.

    ```nodejs
    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });
    ```

* Vytvoří se nová databáze.

    ```nodejs
    client.createDatabase(config.database, (err, created) => {
        if (err) reject(err)
        else resolve(created);
    });
    ```

* Vytvoří se nová kolekce.

    ```nodejs
    client.createCollection(databaseUrl, config.collection, { offerThroughput: 400 }, (err, created) => {
        if (err) reject(err)
        else resolve(created);
    });
    ```

* Vytvoří se některé dokumenty.

    ```nodejs
    client.createDocument(collectionUrl, document, (err, created) => {
        if (err) reject(err)
        else resolve(created);
    });
    ```

* Provede se dotaz SQL přes JSON.

    ```nodejs
    client.queryDocuments(
        collectionUrl,
        'SELECT VALUE r.children FROM root r WHERE r.lastName = "Andersen"'
    ).toArray((err, results) => {
        if (err) reject(err)
        else {
            for (var queryResult of results) {
                let resultString = JSON.stringify(queryResult);
                console.log(`\tQuery returned ${resultString}`);
            }
            console.log();
            resolve(results);
        }
    });
    ```    

## <a name="update-your-connection-string"></a>Aktualizace připojovacího řetězce

Nyní přejděte zpět toohello Azure portálu tooget vaše informace o připojovacím řetězci a zkopírujte jej do aplikace hello.

1. V hello [portál Azure](http://portal.azure.com/), ve vašem Azure Cosmos DB účet, klikněte v levé navigační hello **klíče**a potom klikněte na **klíče pro čtení a zápis**. Použijete tlačítka hello kopírovat na pravé straně hello hello obrazovky toocopy hello URI a Primary Key do hello `config.js` souboru v dalším kroku hello.

    ![Zobrazení a zkopírování přístupový klíč v hello portálu Azure, okna klíče](./media/create-documentdb-dotnet/keys.png)

2. V otevřené hello `config.js` souboru. 

3. Zkopírujte URI hodnota z portálu hello (pomocí tlačítka kopírování hello) a nastavit jej jako hello hodnotu klíče hello koncového bodu v `config.js`. 

    `config.endpoint = "https://FILLME.documents.azure.com"`

4. Poté zkopírujte primární klíč hodnota z portálu hello a nastavit jej jako hello hodnotu hello `config.primaryKey` v `config.js`. Jste nyní aktualizovat vaši aplikaci s všechny údaje hello potřebuje toocommunicate s Azure Cosmos DB. 

    `config.primaryKey "FILLME"`
    
## <a name="run-hello-app"></a>Spuštění aplikace hello
1. Spustit `npm install` v terminálu tooinstall požadované moduly npm

2. Spustit `node app.js` v terminálu toostart aplikace uzlu.

Teď můžete přejít zpět tooData Průzkumníka a zobrazit dotaz, upravit a pracovat s Tato nová data. 

## <a name="review-slas-in-hello-azure-portal"></a>Zkontrolujte SLA v hello portálu Azure

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud ale nebudete toocontinue toouse této aplikace, odstraňte všechny prostředky, které jsou vytvořené tento rychlý start v hello portál Azure s hello následující kroky:

1. V levé nabídce hello v hello portálu Azure klikněte na **skupiny prostředků** a pak klikněte na název hello hello prostředků, které jste vytvořili. 
2. Na stránce skupiny prostředků, klikněte na tlačítko **odstranit**hello textového pole zadejte název hello toodelete hello prostředků a pak klikněte na tlačítko **odstranit**.

## <a name="next-steps"></a>Další kroky

V tento rychlý start když jste se naučili jak toocreate účtu Azure Cosmos DB, vytvořte kolekci pomocí hello Průzkumníku dat a spusťte aplikaci. Nyní můžete importovat další data tooyour Cosmos DB účtu. 

> [!div class="nextstepaction"]
> [Importování dat do služby Azure Cosmos DB](import-data.md)


