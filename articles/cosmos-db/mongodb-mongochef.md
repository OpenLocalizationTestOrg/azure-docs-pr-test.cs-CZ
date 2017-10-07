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
# <a name="use-mongochef-with-an-azure-cosmos-db-api-for-mongodb-account"></a><span data-ttu-id="f79bb-104">Pomocí Azure DB Cosmos MongoChef: rozhraní API pro MongoDB účet</span><span class="sxs-lookup"><span data-stu-id="f79bb-104">Use MongoChef with an Azure Cosmos DB: API for MongoDB account</span></span>

<span data-ttu-id="f79bb-105">tooconnect tooan Cosmos databázi Azure: rozhraní API pro MongoDB účet, musíte:</span><span class="sxs-lookup"><span data-stu-id="f79bb-105">tooconnect tooan Azure Cosmos DB: API for MongoDB account, you must:</span></span>

* <span data-ttu-id="f79bb-106">Stáhněte a nainstalujte [MongoChef](http://3t.io/mongochef)</span><span class="sxs-lookup"><span data-stu-id="f79bb-106">Download and install [MongoChef](http://3t.io/mongochef)</span></span>
* <span data-ttu-id="f79bb-107">Mít vaše Azure DB Cosmos: rozhraní API pro MongoDB účet [připojovací řetězec](connect-mongodb-account.md) informace</span><span class="sxs-lookup"><span data-stu-id="f79bb-107">Have your Azure Cosmos DB: API for MongoDB account [connection string](connect-mongodb-account.md) information</span></span>

## <a name="create-hello-connection-in-mongochef"></a><span data-ttu-id="f79bb-108">Vytvoření připojení hello v MongoChef</span><span class="sxs-lookup"><span data-stu-id="f79bb-108">Create hello connection in MongoChef</span></span>
<span data-ttu-id="f79bb-109">tooadd vaše Azure DB Cosmos: rozhraní API pro MongoDB účet toohello MongoChef Správce připojení, proveďte následující kroky hello.</span><span class="sxs-lookup"><span data-stu-id="f79bb-109">tooadd your Azure Cosmos DB: API for MongoDB account toohello MongoChef connection manager, perform hello following steps.</span></span>

1. <span data-ttu-id="f79bb-110">Načíst vaše Azure DB Cosmos: rozhraní API pro informace o připojení MongoDB pomocí pokynů hello [zde](connect-mongodb-account.md).</span><span class="sxs-lookup"><span data-stu-id="f79bb-110">Retrieve your Azure Cosmos DB: API for MongoDB connection information using hello instructions [here](connect-mongodb-account.md).</span></span>

    ![Snímek obrazovky okna řetězec připojení hello](./media/mongodb-mongochef/ConnectionStringBlade.png)
2. <span data-ttu-id="f79bb-112">Klikněte na tlačítko **připojit** tooopen hello Správce připojení a pak klikněte na **nové připojení**</span><span class="sxs-lookup"><span data-stu-id="f79bb-112">Click **Connect** tooopen hello Connection Manager, then click **New Connection**</span></span>

    ![Snímek obrazovky Správce připojení MongoChef hello](./media/mongodb-mongochef/ConnectionManager.png)
3. <span data-ttu-id="f79bb-114">V hello **nové připojení** okně na hello **Server** zadejte hello hostitele (FQDN) hello Azure Cosmos DB: rozhraní API pro účet a hello PORT MongoDB.</span><span class="sxs-lookup"><span data-stu-id="f79bb-114">In hello **New Connection** window, on hello **Server** tab, enter hello HOST (FQDN) of hello Azure Cosmos DB: API for MongoDB account and hello PORT.</span></span>

    ![Snímek obrazovky s hello MongoChef připojení správce serveru kartou](./media/mongodb-mongochef/ConnectionManagerServerTab.png)
4. <span data-ttu-id="f79bb-116">V hello **nové připojení** okně na hello **ověřování** , zvolte režim ověřování **Standard (MONGODB CR nebo SCARM-SHA-1)** a zadejte hello uživatelské jméno a HESLO.</span><span class="sxs-lookup"><span data-stu-id="f79bb-116">In hello **New Connection** window, on hello **Authentication** tab, choose Authentication Mode **Standard (MONGODB-CR or SCARM-SHA-1)** and enter hello USERNAME and PASSWORD.</span></span>  <span data-ttu-id="f79bb-117">Přijmout hello výchozí ověřování db (správce) nebo zadejte vlastní hodnotu.</span><span class="sxs-lookup"><span data-stu-id="f79bb-117">Accept hello default authentication db (admin) or provide your own value.</span></span>

    ![Snímek obrazovky s hello MongoChef připojení Správce ověřování kartou](./media/mongodb-mongochef/ConnectionManagerAuthenticationTab.png)
5. <span data-ttu-id="f79bb-119">V hello **nové připojení** okně na hello **SSL** kartě, zkontrolujte hello **používejte protokol SSL protokol tooconnect** zaškrtávací políčko a hello **přijmout serveru SSL podepsaných svým držitelem certifikáty** přepínač.</span><span class="sxs-lookup"><span data-stu-id="f79bb-119">In hello **New Connection** window, on hello **SSL** tab, check hello **Use SSL protocol tooconnect** check box and hello **Accept server self-signed SSL certificates** radio button.</span></span>

    ![Snímek obrazovky s kartou SSL hello MongoChef připojení správce](./media/mongodb-mongochef/ConnectionManagerSSLTab.png)
6. <span data-ttu-id="f79bb-121">Klikněte na tlačítko hello **Test připojení** tlačítko informace o připojení hello toovalidate, klikněte na tlačítko **OK** tooreturn toohello nové připojení okno a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f79bb-121">Click hello **Test Connection** button toovalidate hello connection information, click **OK** tooreturn toohello New Connection window, and then click **Save**.</span></span>

    ![Snímek obrazovky okna připojení testovací MongoChef hello](./media/mongodb-mongochef/TestConnectionResults.png)

## <a name="use-mongochef-toocreate-a-database-collection-and-documents"></a><span data-ttu-id="f79bb-123">Použít MongoChef toocreate databáze, kolekce a dokumentů</span><span class="sxs-lookup"><span data-stu-id="f79bb-123">Use MongoChef toocreate a database, collection, and documents</span></span>
<span data-ttu-id="f79bb-124">toocreate databáze, kolekce a dokumenty pomocí MongoChef, proveďte následující kroky hello.</span><span class="sxs-lookup"><span data-stu-id="f79bb-124">toocreate a database, collection, and documents using MongoChef, perform hello following steps.</span></span>

1. <span data-ttu-id="f79bb-125">V **Správce připojení**, zvýrazněte hello připojení a klikněte na **Connect**.</span><span class="sxs-lookup"><span data-stu-id="f79bb-125">In **Connection Manager**, highlight hello connection and click **Connect**.</span></span>

    ![Snímek obrazovky Správce připojení MongoChef hello](./media/mongodb-mongochef/ConnectToAccount.png)
2. <span data-ttu-id="f79bb-127">Klikněte pravým tlačítkem na hostitele hello a zvolte **přidat databázi**.</span><span class="sxs-lookup"><span data-stu-id="f79bb-127">Right click hello host and choose **Add Database**.</span></span>  <span data-ttu-id="f79bb-128">Zadejte název databáze a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="f79bb-128">Provide a database name and click **OK**.</span></span>

    ![Snímek obrazovky hello možnost MongoChef přidat databáze](./media/mongodb-mongochef/AddDatabase1.png)
3. <span data-ttu-id="f79bb-130">Klikněte pravým tlačítkem na databázi hello a zvolte **přidat kolekce**.</span><span class="sxs-lookup"><span data-stu-id="f79bb-130">Right click hello database and choose **Add Collection**.</span></span>  <span data-ttu-id="f79bb-131">Zadejte název kolekce a klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f79bb-131">Provide a collection name and click **Create**.</span></span>

    ![Snímek obrazovky hello možnost MongoChef přidat kolekce](./media/mongodb-mongochef/AddCollection.png)
4. <span data-ttu-id="f79bb-133">Klikněte na tlačítko hello **kolekce** nabídky položky, klepněte na tlačítko **přidat dokument**.</span><span class="sxs-lookup"><span data-stu-id="f79bb-133">Click hello **Collection** menu item, then click **Add Document**.</span></span>

    ![Snímek obrazovky položku nabídky přidat dokument MongoChef hello](./media/mongodb-mongochef/AddDocument1.png)
5. <span data-ttu-id="f79bb-135">V dialogovém okně Přidat dokument hello, vložte následující hello a pak klikněte na tlačítko **přidat dokument**.</span><span class="sxs-lookup"><span data-stu-id="f79bb-135">In hello Add Document dialog, paste hello following and then click **Add Document**.</span></span>

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
6. <span data-ttu-id="f79bb-136">Přidání jiného dokumentu, tentokrát s hello následující obsah.</span><span class="sxs-lookup"><span data-stu-id="f79bb-136">Add another document, this time with hello following content.</span></span>

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
7. <span data-ttu-id="f79bb-137">Spustit ukázkový dotaz.</span><span class="sxs-lookup"><span data-stu-id="f79bb-137">Execute a sample query.</span></span> <span data-ttu-id="f79bb-138">Například vyhledejte rodiny s hello příjmení 'rodinu a nadřazené položky návratový hello a stav pole.</span><span class="sxs-lookup"><span data-stu-id="f79bb-138">For example, search for families with hello last name 'Andersen' and return hello parents and state fields.</span></span>

    ![Snímek obrazovky Mongo Chef výsledky dotazu](./media/mongodb-mongochef/QueryDocument1.png)

## <a name="next-steps"></a><span data-ttu-id="f79bb-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f79bb-140">Next steps</span></span>
* <span data-ttu-id="f79bb-141">Prozkoumejte Azure Cosmos DB: Rozhraní API pro MongoDB [ukázky](mongodb-samples.md).</span><span class="sxs-lookup"><span data-stu-id="f79bb-141">Explore Azure Cosmos DB: API for MongoDB [samples](mongodb-samples.md).</span></span>
