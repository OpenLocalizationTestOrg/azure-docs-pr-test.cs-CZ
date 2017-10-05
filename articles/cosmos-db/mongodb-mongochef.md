---
title: "Použít MongoChef pro Azure Cosmos DB | Microsoft Docs"
description: "Další informace o použití MongoChef se Azure DB Cosmos: rozhraní API pro MongoDB účet"
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
ms.openlocfilehash: 54c9799bd646b827f602e2ea2f9a15a4fc853f00
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-mongochef-with-an-azure-cosmos-db-api-for-mongodb-account"></a><span data-ttu-id="e6319-104">Pomocí Azure DB Cosmos MongoChef: rozhraní API pro MongoDB účet</span><span class="sxs-lookup"><span data-stu-id="e6319-104">Use MongoChef with an Azure Cosmos DB: API for MongoDB account</span></span>

<span data-ttu-id="e6319-105">Pro připojení k databázi Azure Cosmos: rozhraní API pro MongoDB účet, musíte:</span><span class="sxs-lookup"><span data-stu-id="e6319-105">To connect to an Azure Cosmos DB: API for MongoDB account, you must:</span></span>

* <span data-ttu-id="e6319-106">Stáhněte a nainstalujte [MongoChef](http://3t.io/mongochef)</span><span class="sxs-lookup"><span data-stu-id="e6319-106">Download and install [MongoChef](http://3t.io/mongochef)</span></span>
* <span data-ttu-id="e6319-107">Mít vaše Azure DB Cosmos: rozhraní API pro MongoDB účet [připojovací řetězec](connect-mongodb-account.md) informace</span><span class="sxs-lookup"><span data-stu-id="e6319-107">Have your Azure Cosmos DB: API for MongoDB account [connection string](connect-mongodb-account.md) information</span></span>

## <a name="create-the-connection-in-mongochef"></a><span data-ttu-id="e6319-108">Vytvoření připojení v MongoChef</span><span class="sxs-lookup"><span data-stu-id="e6319-108">Create the connection in MongoChef</span></span>
<span data-ttu-id="e6319-109">Přidání vaší Azure DB Cosmos: rozhraní API pro MongoDB účet pro připojení správce MongoChef, proveďte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="e6319-109">To add your Azure Cosmos DB: API for MongoDB account to the MongoChef connection manager, perform the following steps.</span></span>

1. <span data-ttu-id="e6319-110">Načíst vaše Azure DB Cosmos: rozhraní API pro informace o připojení MongoDB pomocí pokynů [zde](connect-mongodb-account.md).</span><span class="sxs-lookup"><span data-stu-id="e6319-110">Retrieve your Azure Cosmos DB: API for MongoDB connection information using the instructions [here](connect-mongodb-account.md).</span></span>

    ![Snímek obrazovky okna řetězec připojení](./media/mongodb-mongochef/ConnectionStringBlade.png)
2. <span data-ttu-id="e6319-112">Klikněte na tlačítko **připojit** a otevřít Správce připojení, potom klikněte na **nové připojení**</span><span class="sxs-lookup"><span data-stu-id="e6319-112">Click **Connect** to open the Connection Manager, then click **New Connection**</span></span>

    ![Snímek obrazovky Správce připojení MongoChef](./media/mongodb-mongochef/ConnectionManager.png)
3. <span data-ttu-id="e6319-114">V **nové připojení** okno na **Server** zadejte hostitele (FQDN) systému Cosmos databázi Azure: rozhraní API pro MongoDB účet a PORT.</span><span class="sxs-lookup"><span data-stu-id="e6319-114">In the **New Connection** window, on the **Server** tab, enter the HOST (FQDN) of the Azure Cosmos DB: API for MongoDB account and the PORT.</span></span>

    ![Snímek obrazovky na kartu server manager MongoChef připojení](./media/mongodb-mongochef/ConnectionManagerServerTab.png)
4. <span data-ttu-id="e6319-116">V **nové připojení** okno na **ověřování** , zvolte režim ověřování **Standard (MONGODB CR nebo SCARM-SHA-1)** a zadejte uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="e6319-116">In the **New Connection** window, on the **Authentication** tab, choose Authentication Mode **Standard (MONGODB-CR or SCARM-SHA-1)** and enter the USERNAME and PASSWORD.</span></span>  <span data-ttu-id="e6319-117">Přijměte výchozí ověřování databáze (správce) nebo zadejte vlastní hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e6319-117">Accept the default authentication db (admin) or provide your own value.</span></span>

    ![Snímek obrazovky s kartou ověřování MongoChef připojení správce](./media/mongodb-mongochef/ConnectionManagerAuthenticationTab.png)
5. <span data-ttu-id="e6319-119">V **nové připojení** okno na **SSL** zkontrolujte **protokol SSL používá pro připojení** zaškrtávací políčko a **přijmout SSL certifikáty podepsané svým držitelem serveru**  přepínač.</span><span class="sxs-lookup"><span data-stu-id="e6319-119">In the **New Connection** window, on the **SSL** tab, check the **Use SSL protocol to connect** check box and the **Accept server self-signed SSL certificates** radio button.</span></span>

    ![Snímek obrazovky s kartou SSL MongoChef připojení správce](./media/mongodb-mongochef/ConnectionManagerSSLTab.png)
6. <span data-ttu-id="e6319-121">Klikněte **Test připojení** a ověřte informace o připojení, klikněte na tlačítko **OK** vraťte do okna nové připojení, a pak klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e6319-121">Click the **Test Connection** button to validate the connection information, click **OK** to return to the New Connection window, and then click **Save**.</span></span>

    ![Snímek obrazovky okna MongoChef test připojení](./media/mongodb-mongochef/TestConnectionResults.png)

## <a name="use-mongochef-to-create-a-database-collection-and-documents"></a><span data-ttu-id="e6319-123">Použít MongoChef k vytvoření databáze, kolekce a dokumentů</span><span class="sxs-lookup"><span data-stu-id="e6319-123">Use MongoChef to create a database, collection, and documents</span></span>
<span data-ttu-id="e6319-124">K vytvoření databáze, kolekce a dokumenty pomocí MongoChef, proveďte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="e6319-124">To create a database, collection, and documents using MongoChef, perform the following steps.</span></span>

1. <span data-ttu-id="e6319-125">V **Správce připojení**, zvýrazněte připojení a klikněte na **Connect**.</span><span class="sxs-lookup"><span data-stu-id="e6319-125">In **Connection Manager**, highlight the connection and click **Connect**.</span></span>

    ![Snímek obrazovky Správce připojení MongoChef](./media/mongodb-mongochef/ConnectToAccount.png)
2. <span data-ttu-id="e6319-127">Klikněte pravým tlačítkem na hostitele a zvolte **přidat databázi**.</span><span class="sxs-lookup"><span data-stu-id="e6319-127">Right click the host and choose **Add Database**.</span></span>  <span data-ttu-id="e6319-128">Zadejte název databáze a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e6319-128">Provide a database name and click **OK**.</span></span>

    ![Snímek obrazovky možnost MongoChef přidat databáze](./media/mongodb-mongochef/AddDatabase1.png)
3. <span data-ttu-id="e6319-130">Klikněte pravým tlačítkem na databázi a zvolte **přidat kolekce**.</span><span class="sxs-lookup"><span data-stu-id="e6319-130">Right click the database and choose **Add Collection**.</span></span>  <span data-ttu-id="e6319-131">Zadejte název kolekce a klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e6319-131">Provide a collection name and click **Create**.</span></span>

    ![Snímek obrazovky možnost MongoChef přidat kolekce](./media/mongodb-mongochef/AddCollection.png)
4. <span data-ttu-id="e6319-133">Klikněte na tlačítko **kolekce** nabídky položky, klepněte na tlačítko **přidat dokument**.</span><span class="sxs-lookup"><span data-stu-id="e6319-133">Click the **Collection** menu item, then click **Add Document**.</span></span>

    ![Snímek obrazovky MongoChef přidat dokument položky nabídky](./media/mongodb-mongochef/AddDocument1.png)
5. <span data-ttu-id="e6319-135">V dialogovém okně Přidat dokument vložte následující a potom klikněte na **přidat dokument**.</span><span class="sxs-lookup"><span data-stu-id="e6319-135">In the Add Document dialog, paste the following and then click **Add Document**.</span></span>

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
6. <span data-ttu-id="e6319-136">Přidání jiného dokumentu, tentokrát s následujícím obsahem.</span><span class="sxs-lookup"><span data-stu-id="e6319-136">Add another document, this time with the following content.</span></span>

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
7. <span data-ttu-id="e6319-137">Spustit ukázkový dotaz.</span><span class="sxs-lookup"><span data-stu-id="e6319-137">Execute a sample query.</span></span> <span data-ttu-id="e6319-138">Například vyhledejte rodiny s příjmení 'rodinu a vrátí nadřazené položky a pole stavu.</span><span class="sxs-lookup"><span data-stu-id="e6319-138">For example, search for families with the last name 'Andersen' and return the parents and state fields.</span></span>

    ![Snímek obrazovky Mongo Chef výsledky dotazu](./media/mongodb-mongochef/QueryDocument1.png)

## <a name="next-steps"></a><span data-ttu-id="e6319-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e6319-140">Next steps</span></span>
* <span data-ttu-id="e6319-141">Prozkoumejte Azure Cosmos DB: Rozhraní API pro MongoDB [ukázky](mongodb-samples.md).</span><span class="sxs-lookup"><span data-stu-id="e6319-141">Explore Azure Cosmos DB: API for MongoDB [samples](mongodb-samples.md).</span></span>
