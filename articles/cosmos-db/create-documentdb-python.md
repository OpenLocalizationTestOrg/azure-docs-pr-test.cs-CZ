---
title: "Azure Cosmos DB: Sestavení aplikace pomocí Python a hello rozhraní API DocumentDB | Microsoft Docs"
description: "Uvede ukázku kódu Python, můžete použít dotaz tooand tooconnect hello rozhraní API služby Azure Cosmos databáze DocumentDB"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 51c11be2-af6d-425f-a86a-39cbfe61da29
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: hero-article
ms.date: 05/13/2017
ms.author: mimig
ms.openlocfilehash: e66965ab493c6ef693e88a3767a401d39e1bde2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-documentdb-api-app-with-python-and-hello-azure-portal"></a>Azure Cosmos DB: Sestavení aplikace DocumentDB API s Pythonem a hello portálu Azure

Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku. Můžete rychle vytvořit a dotazovat dokumentu, klíč/hodnota a graf databází, které těžit z globální distribuční hello a možnosti vodorovné škálování jádrem hello Azure Cosmos DB. 

Tento rychlý start předvádí, jak hello toocreate účet Azure Cosmos DB, dokumentu databáze a kolekce pomocí portálu Azure. Potom sestavení a spuštění konzoly aplikace založená na hello [DocumentDB Python API](documentdb-sdk-python.md).

## <a name="prerequisites"></a>Požadavky

* Před spuštěním této ukázce, musíte mít hello následující požadavky:
    * [Visual Studio 2015](http://www.visualstudio.com/) nebo vyšší.
    * Python Tools for Visual Studio z [GitHubu](http://microsoft.github.io/PTVS/). V tomto kurzu se používá Python Tools for VS 2015.
    * Python 2.7 z webu [python.org](https://www.python.org/downloads/release/python-2712/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Vytvoření účtu databáze

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a>Přidání kolekce

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="clone-hello-sample-application"></a>Klonování hello ukázkové aplikace

Teď umožňuje nastavit připojovací řetězec hello klonování DocumentDB API aplikace z githubu a potom ho spusťte. Uvidíte, jak je snadné toowork s daty prostřednictvím kódu programu. 

1. Otevřete okno terminálu git, jako je například git bash a `cd` tooa pracovní adresář.  

2. Spusťte následující příkaz tooclone hello Ukázka úložiště hello. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-python-getting-started.git
    ```  
## <a name="review-hello-code"></a>Zkontrolujte hello kódu

Provedeme jejich stručný přehled o dění v aplikaci hello. Otevřete hello DocumentDBGetStarted.py souboru a najdete v ní vytvořit tyto řádky kódu hello prostředky Azure Cosmos DB. 


* Hello DocumentClient je inicializován.

    ```python
    # Initialize hello Python DocumentDB client
    client = document_client.DocumentClient(config['ENDPOINT'], {'masterKey': config['MASTERKEY']})
    ```

* Vytvoří se nová databáze.

    ```python
    # Create a database
    db = client.CreateDatabase({ 'id': config['DOCUMENTDB_DATABASE'] })
    ```

* Vytvoří se nová kolekce.

    ```python
    # Create collection options
    options = {
        'offerEnableRUPerMinuteThroughput': True,
        'offerVersion': "V2",
        'offerThroughput': 400
    }

    # Create a collection
    collection = client.CreateCollection(db['_self'], { 'id': config['DOCUMENTDB_COLLECTION'] }, options)
    ```

* Vytvoří se některé dokumenty.

    ```python
    # Create some documents
    document1 = client.CreateDocument(collection['_self'],
        { 
            'id': 'server1',
            'Web Site': 0,
            'Cloud Service': 0,
            'Virtual Machine': 0,
            'name': 'some' 
        })
    ```

* Provede se dotaz v jazyce SQL.

    ```python
    # Query them in SQL
    query = { 'query': 'SELECT * FROM server s' }    
            
    options = {} 
    options['enableCrossPartitionQuery'] = True
    options['maxItemCount'] = 2

    result_iterable = client.QueryDocuments(collection['_self'], query, options)
    results = list(result_iterable);

    print(results)
    ```

## <a name="update-your-connection-string"></a>Aktualizace připojovacího řetězce

Nyní přejděte zpět toohello Azure portálu tooget vaše informace o připojovacím řetězci a zkopírujte jej do aplikace hello.

1. V hello [portál Azure](http://portal.azure.com/), ve vašem Azure Cosmos DB účet, klikněte v levé navigační hello **klíče**a potom klikněte na **klíče pro čtení a zápis**. Použijete tlačítka hello kopírovat na pravé straně hello hello obrazovky toocopy hello URI a Primary Key do hello `DocumentDBGetStarted.py` souboru v dalším kroku hello.

    ![Zobrazení a zkopírování přístupový klíč v hello portálu Azure, okna klíče](./media/create-documentdb-dotnet/keys.png)

2. V otevřené hello `DocumentDBGetStarted.py` souboru. 

3. Zkopírujte URI hodnota z portálu hello (pomocí tlačítka kopírování hello) a nastavit jej jako hello hodnotu klíče hello koncového bodu v `DocumentDBGetStarted.py`. 

    `config.ENDPOINT : "https://FILLME.documents.azure.com"`

4. Poté zkopírujte primární klíč hodnota z portálu hello a nastavit jej jako hello hodnotu hello `config.MASTERKEY` v `DocumentDBGetStarted.py`. Jste nyní aktualizovat vaši aplikaci s všechny údaje hello potřebuje toocommunicate s Azure Cosmos DB. 

    `config.MASTERKEY : "FILLME"`
    
## <a name="run-hello-app"></a>Spuštění aplikace hello
1. V sadě Visual Studio, klikněte pravým tlačítkem na projekt hello v **Průzkumníku řešení**vyberte hello aktuální prostředí Python a pak klikněte pravým tlačítkem.

2. Vyberte Možnost Instalovat balíček Pythonu a potom zadejte **pydocumentdb**.

3. Spusťte aplikaci hello toorun F5. Aplikace se zobrazí v prohlížeči. 

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
> [Importovat data do Azure Cosmos DB pro hello DocumentDB rozhraní API](import-data.md)


