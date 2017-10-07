---
title: aaaUse MongoChef pro Azure Cosmos DB | Microsoft Docs
description: "Zjistěte, jak toouse MongoChef se Azure DB Cosmos: rozhraní API pro MongoDB účet"
keywords: mongochef
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 352c5fb9-8772-4c5f-87ac-74885e63ecac
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: anhoh
ms.openlocfilehash: 4b047797b231c34ccc6f2ed02416525c6228d596
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-mongochef-with-an-azure-cosmos-db-api-for-mongodb-account"></a>Pomocí Azure DB Cosmos MongoChef: rozhraní API pro MongoDB účet

tooconnect tooan Cosmos databázi Azure: rozhraní API pro MongoDB účet, musíte:

* Stáhněte a nainstalujte [MongoChef](http://3t.io/mongochef)
* Mít vaše Azure DB Cosmos: rozhraní API pro MongoDB účet [připojovací řetězec](connect-mongodb-account.md) informace

## <a name="create-hello-connection-in-mongochef"></a>Vytvoření připojení hello v MongoChef
tooadd vaše Azure DB Cosmos: rozhraní API pro MongoDB účet toohello MongoChef Správce připojení, proveďte následující kroky hello.

1. Načíst vaše Azure DB Cosmos: rozhraní API pro informace o připojení MongoDB pomocí pokynů hello [zde](connect-mongodb-account.md).

    ![Snímek obrazovky okna řetězec připojení hello](./media/mongodb-mongochef/ConnectionStringBlade.png)
2. Klikněte na tlačítko **připojit** tooopen hello Správce připojení a pak klikněte na **nové připojení**

    ![Snímek obrazovky Správce připojení MongoChef hello](./media/mongodb-mongochef/ConnectionManager.png)
3. V hello **nové připojení** okně na hello **Server** zadejte hello hostitele (FQDN) hello Azure Cosmos DB: rozhraní API pro účet a hello PORT MongoDB.

    ![Snímek obrazovky s hello MongoChef připojení správce serveru kartou](./media/mongodb-mongochef/ConnectionManagerServerTab.png)
4. V hello **nové připojení** okně na hello **ověřování** , zvolte režim ověřování **Standard (MONGODB CR nebo SCARM-SHA-1)** a zadejte hello uživatelské jméno a HESLO.  Přijmout hello výchozí ověřování db (správce) nebo zadejte vlastní hodnotu.

    ![Snímek obrazovky s hello MongoChef připojení Správce ověřování kartou](./media/mongodb-mongochef/ConnectionManagerAuthenticationTab.png)
5. V hello **nové připojení** okně na hello **SSL** kartě, zkontrolujte hello **používejte protokol SSL protokol tooconnect** zaškrtávací políčko a hello **přijmout serveru SSL podepsaných svým držitelem certifikáty** přepínač.

    ![Snímek obrazovky s kartou SSL hello MongoChef připojení správce](./media/mongodb-mongochef/ConnectionManagerSSLTab.png)
6. Klikněte na tlačítko hello **Test připojení** tlačítko informace o připojení hello toovalidate, klikněte na tlačítko **OK** tooreturn toohello nové připojení okno a potom klikněte na **Uložit**.

    ![Snímek obrazovky okna připojení testovací MongoChef hello](./media/mongodb-mongochef/TestConnectionResults.png)

## <a name="use-mongochef-toocreate-a-database-collection-and-documents"></a>Použít MongoChef toocreate databáze, kolekce a dokumentů
toocreate databáze, kolekce a dokumenty pomocí MongoChef, proveďte následující kroky hello.

1. V **Správce připojení**, zvýrazněte hello připojení a klikněte na **Connect**.

    ![Snímek obrazovky Správce připojení MongoChef hello](./media/mongodb-mongochef/ConnectToAccount.png)
2. Klikněte pravým tlačítkem na hostitele hello a zvolte **přidat databázi**.  Zadejte název databáze a klikněte na tlačítko **OK**.

    ![Snímek obrazovky hello možnost MongoChef přidat databáze](./media/mongodb-mongochef/AddDatabase1.png)
3. Klikněte pravým tlačítkem na databázi hello a zvolte **přidat kolekce**.  Zadejte název kolekce a klikněte na **vytvořit**.

    ![Snímek obrazovky hello možnost MongoChef přidat kolekce](./media/mongodb-mongochef/AddCollection.png)
4. Klikněte na tlačítko hello **kolekce** nabídky položky, klepněte na tlačítko **přidat dokument**.

    ![Snímek obrazovky položku nabídky přidat dokument MongoChef hello](./media/mongodb-mongochef/AddDocument1.png)
5. V dialogovém okně Přidat dokument hello, vložte následující hello a pak klikněte na tlačítko **přidat dokument**.

        {
        "_id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
               { "firstName": "Thomas" },
               { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "isRegistered": true
        }
6. Přidání jiného dokumentu, tentokrát s hello následující obsah.

        {
        "_id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam",
                 "givenName": "Jesse",
                "gender": "female", "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            {
                "familyName": "Miller",
                 "givenName": "Lisa",
                 "gender": "female",
                 "grade": 8 }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
        }
7. Spustit ukázkový dotaz. Například vyhledejte rodiny s hello příjmení 'rodinu a nadřazené položky návratový hello a stav pole.

    ![Snímek obrazovky Mongo Chef výsledky dotazu](./media/mongodb-mongochef/QueryDocument1.png)

## <a name="next-steps"></a>Další kroky
* Prozkoumejte Azure Cosmos DB: Rozhraní API pro MongoDB [ukázky](mongodb-samples.md).
