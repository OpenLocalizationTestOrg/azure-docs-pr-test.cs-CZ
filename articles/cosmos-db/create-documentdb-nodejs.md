---
title: "Databáze Azure Cosmos: Sestavení aplikace s kódem Node.js a rozhraním API DocumentDB | Dokumentace Microsoftu"
description: "Představuje ukázku kódu Node.js, který můžete použít k připojení a dotazování do rozhraní API DocumentDB databáze Azure Cosmos."
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
ms.openlocfilehash: 26e3548bf6aacbc60c4c46a5cc88749ca14cec01
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cosmos-db-build-a-documentdb-api-app-with-nodejs-and-the-azure-portal"></a><span data-ttu-id="b142a-103">Databáze Azure Cosmos: Sestavení aplikace s rozhraním API DocumentDB pomocí kódu Node.js a webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b142a-103">Azure Cosmos DB: Build a DocumentDB API app with Node.js and the Azure portal</span></span>

<span data-ttu-id="b142a-104">Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku.</span><span class="sxs-lookup"><span data-stu-id="b142a-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="b142a-105">Můžete snadno vytvořit a dotazovat databáze dotazů, klíčů/hodnot a grafů, které tak můžou využívat výhody použitelnosti v celosvětovém měřítku a možností horizontálního škálování v jádru Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="b142a-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from the global distribution and horizontal scale capabilities at the core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="b142a-106">Tento rychlý start popisuje způsob vytvoření účtu databáze Azure Cosmos, databáze dokumentů a kolekce pomocí webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="b142a-106">This quick start demonstrates how to create an Azure Cosmos DB account, document database, and collection using the Azure portal.</span></span> <span data-ttu-id="b142a-107">Potom sestavíte a spustíte aplikaci konzoly založenou na [rozhraní API Node.js DocumentDB](documentdb-sdk-node.md).</span><span class="sxs-lookup"><span data-stu-id="b142a-107">You then build and run a console app built on the [DocumentDB Node.js API](documentdb-sdk-node.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b142a-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b142a-108">Prerequisites</span></span>

* <span data-ttu-id="b142a-109">Než budete moct tuto ukázku spustit, je potřeba splnit následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="b142a-109">Before you can run this sample, you must have the following prerequisites:</span></span>
    * <span data-ttu-id="b142a-110">[Node.js](https://nodejs.org/en/) verze 0.10.29 nebo vyšší</span><span class="sxs-lookup"><span data-stu-id="b142a-110">[Node.js](https://nodejs.org/en/) version v0.10.29 or higher</span></span>
    * [<span data-ttu-id="b142a-111">Git</span><span class="sxs-lookup"><span data-stu-id="b142a-111">Git</span></span>](http://git-scm.com/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="b142a-112">Vytvoření účtu databáze</span><span class="sxs-lookup"><span data-stu-id="b142a-112">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a><span data-ttu-id="b142a-113">Přidání kolekce</span><span class="sxs-lookup"><span data-stu-id="b142a-113">Add a collection</span></span>

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="clone-the-sample-application"></a><span data-ttu-id="b142a-114">Klonování ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="b142a-114">Clone the sample application</span></span>

<span data-ttu-id="b142a-115">Teď naklonujeme aplikaci s rozhraním API DocumentDB z GitHubu, nastavíme připojovací řetězec a spustíme ji.</span><span class="sxs-lookup"><span data-stu-id="b142a-115">Now let's clone a DocumentDB API app from github, set the connection string, and run it.</span></span> <span data-ttu-id="b142a-116">Uvidíte, jak snadno se pracuje s daty prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="b142a-116">You see how easy it is to work with data programmatically.</span></span> 

1. <span data-ttu-id="b142a-117">Otevřete okno terminálu Git, jako je třeba Git Bash, a pomocí `CD` přejděte do pracovního adresáře.</span><span class="sxs-lookup"><span data-stu-id="b142a-117">Open a git terminal window, such as git bash, and `CD` to a working directory.</span></span>  

2. <span data-ttu-id="b142a-118">Ukázkové úložiště naklonujete spuštěním následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="b142a-118">Run the following command to clone the sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-nodejs-getting-started.git
    ```

## <a name="review-the-code"></a><span data-ttu-id="b142a-119">Kontrola kódu</span><span class="sxs-lookup"><span data-stu-id="b142a-119">Review the code</span></span>

<span data-ttu-id="b142a-120">Ještě jednou se stručně podívejme na to, co se v aplikaci děje.</span><span class="sxs-lookup"><span data-stu-id="b142a-120">Let's make a quick review of what's happening in the app.</span></span> <span data-ttu-id="b142a-121">Otevřete soubor `app.js` a zjistíte, že tyto řádky kódu vytvářejí prostředky databáze Azure Cosmos.</span><span class="sxs-lookup"><span data-stu-id="b142a-121">Open the `app.js` file and you find that these lines of code create the Azure Cosmos DB resources.</span></span> 

* <span data-ttu-id="b142a-122">Inicializuje se `documentClient`.</span><span class="sxs-lookup"><span data-stu-id="b142a-122">The `documentClient` is initialized.</span></span>

    ```nodejs
    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });
    ```

* <span data-ttu-id="b142a-123">Vytvoří se nová databáze.</span><span class="sxs-lookup"><span data-stu-id="b142a-123">A new database is created.</span></span>

    ```nodejs
    client.createDatabase(config.database, (err, created) => {
        if (err) reject(err)
        else resolve(created);
    });
    ```

* <span data-ttu-id="b142a-124">Vytvoří se nová kolekce.</span><span class="sxs-lookup"><span data-stu-id="b142a-124">A new collection is created.</span></span>

    ```nodejs
    client.createCollection(databaseUrl, config.collection, { offerThroughput: 400 }, (err, created) => {
        if (err) reject(err)
        else resolve(created);
    });
    ```

* <span data-ttu-id="b142a-125">Vytvoří se některé dokumenty.</span><span class="sxs-lookup"><span data-stu-id="b142a-125">Some documents are created.</span></span>

    ```nodejs
    client.createDocument(collectionUrl, document, (err, created) => {
        if (err) reject(err)
        else resolve(created);
    });
    ```

* <span data-ttu-id="b142a-126">Provede se dotaz SQL přes JSON.</span><span class="sxs-lookup"><span data-stu-id="b142a-126">A SQL query over JSON is performed.</span></span>

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

## <a name="update-your-connection-string"></a><span data-ttu-id="b142a-127">Aktualizace připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="b142a-127">Update your connection string</span></span>

<span data-ttu-id="b142a-128">Teď se vraťte zpátky na portál Azure Portal, kde najdete informace o připojovacím řetězci, a zkopírujte je do aplikace.</span><span class="sxs-lookup"><span data-stu-id="b142a-128">Now go back to the Azure portal to get your connection string information and copy it into the app.</span></span>

1. <span data-ttu-id="b142a-129">Na webu [Azure Portal](http://portal.azure.com/) klikněte v účtu databáze Azure Cosmos v levém navigačním panelu na možnost **Klíče** a potom klikněte na **Klíče pro čtení i zápis**.</span><span class="sxs-lookup"><span data-stu-id="b142a-129">In the [Azure portal](http://portal.azure.com/), in your Azure Cosmos DB account, in the left navigation click **Keys**, and then click **Read-write Keys**.</span></span> <span data-ttu-id="b142a-130">V dalším kroku zkopírujete pomocí tlačítek kopírování na pravé straně obrazovky identifikátor URI a primární klíč do souboru `config.js`.</span><span class="sxs-lookup"><span data-stu-id="b142a-130">You'll use the copy buttons on the right side of the screen to copy the URI and Primary Key into the `config.js` file in the next step.</span></span>

    ![Zobrazení a zkopírování přístupového klíče na webu Azure Portal v okně Klíče](./media/create-documentdb-dotnet/keys.png)

2. <span data-ttu-id="b142a-132">Otevřete soubor `config.js`.</span><span class="sxs-lookup"><span data-stu-id="b142a-132">In Open the `config.js` file.</span></span> 

3. <span data-ttu-id="b142a-133">Z portálu zkopírujte hodnotu identifikátoru URI (pomocí tlačítka kopírování) a nastavte ji jako hodnotu klíče koncového bodu v souboru `config.js`.</span><span class="sxs-lookup"><span data-stu-id="b142a-133">Copy your URI value from the portal (using the copy button) and make it the value of the endpoint key in `config.js`.</span></span> 

    `config.endpoint = "https://FILLME.documents.azure.com"`

4. <span data-ttu-id="b142a-134">Potom z portálu zkopírujte hodnotu PRIMÁRNÍHO KLÍČE a nastavte ji jako hodnotu `config.primaryKey` v souboru `config.js`.</span><span class="sxs-lookup"><span data-stu-id="b142a-134">Then copy your PRIMARY KEY value from the portal and make it the value of the `config.primaryKey` in `config.js`.</span></span> <span data-ttu-id="b142a-135">Teď jste aktualizovali aplikaci a zadali do ní všechny informace potřebné ke komunikaci s databází Azure Cosmos.</span><span class="sxs-lookup"><span data-stu-id="b142a-135">You've now updated your app with all the info it needs to communicate with Azure Cosmos DB.</span></span> 

    `config.primaryKey "FILLME"`
    
## <a name="run-the-app"></a><span data-ttu-id="b142a-136">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="b142a-136">Run the app</span></span>
1. <span data-ttu-id="b142a-137">Spusťte v terminálu `npm install`, aby se nainstalovaly požadované moduly NPM.</span><span class="sxs-lookup"><span data-stu-id="b142a-137">Run `npm install` in a terminal to install required npm modules</span></span>

2. <span data-ttu-id="b142a-138">Spuštění v terminálu `node app.js`, aby se spustila aplikace uzlu.</span><span class="sxs-lookup"><span data-stu-id="b142a-138">Run `node app.js` in a terminal to start your node application.</span></span>

<span data-ttu-id="b142a-139">Teď se můžete vrátit do Průzkumníku dat a zobrazit dotaz nebo provést úpravy a pracovat s těmito novými daty.</span><span class="sxs-lookup"><span data-stu-id="b142a-139">You can now go back to Data Explorer and see query, modify, and work with this new data.</span></span> 

## <a name="review-slas-in-the-azure-portal"></a><span data-ttu-id="b142a-140">Ověření podmínek SLA na portálu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b142a-140">Review SLAs in the Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="b142a-141">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="b142a-141">Clean up resources</span></span>

<span data-ttu-id="b142a-142">Pokud nebudete tuto aplikace nadále používat, odstraňte na základě následujícího postupu z portálu Azure Portal všechny prostředky vytvořené podle tohoto rychlého startu:</span><span class="sxs-lookup"><span data-stu-id="b142a-142">If you're not going to continue to use this app, delete all resources created by this quickstart in the Azure portal with the following steps:</span></span>

1. <span data-ttu-id="b142a-143">V nabídce vlevo na portálu Azure Portal klikněte na **Skupiny prostředků** a pak klikněte na název vytvořeného prostředku.</span><span class="sxs-lookup"><span data-stu-id="b142a-143">From the left-hand menu in the Azure portal, click **Resource groups** and then click the name of the resource you created.</span></span> 
2. <span data-ttu-id="b142a-144">Na stránce skupiny prostředků klikněte na **Odstranit**, do textového pole zadejte prostředek, který chcete odstranit, a pak klikněte na **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="b142a-144">On your resource group page, click **Delete**, type the name of the resource to delete in the text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b142a-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b142a-145">Next steps</span></span>

<span data-ttu-id="b142a-146">V tomto rychlém startu jsme se seznámili s postupem vytvoření účtu databáze Azure Cosmos, vytvoření kolekce pomocí Průzkumníka dat a spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="b142a-146">In this quickstart, you've learned how to create an Azure Cosmos DB account, create a collection using the Data Explorer, and run an app.</span></span> <span data-ttu-id="b142a-147">Teď můžete do účtu databáze Cosmos importovat další data.</span><span class="sxs-lookup"><span data-stu-id="b142a-147">You can now import additional data to your Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="b142a-148">Importování dat do služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="b142a-148">Import data into Azure Cosmos DB</span></span>](import-data.md)


