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
# <a name="azure-cosmos-db-build-a-documentdb-api-app-with-nodejs-and-hello-azure-portal"></a><span data-ttu-id="d3fbe-103">Azure Cosmos DB: Sestavení aplikace DocumentDB rozhraní API pomocí Node.js a hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="d3fbe-103">Azure Cosmos DB: Build a DocumentDB API app with Node.js and hello Azure portal</span></span>

<span data-ttu-id="d3fbe-104">Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku.</span><span class="sxs-lookup"><span data-stu-id="d3fbe-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="d3fbe-105">Můžete rychle vytvořit a dotazovat dokumentu, klíč/hodnota a graf databází, které těžit z globální distribuční hello a možnosti vodorovné škálování jádrem hello Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="d3fbe-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="d3fbe-106">Tento rychlý start předvádí, jak hello toocreate účet Azure Cosmos DB, dokumentu databáze a kolekce pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d3fbe-106">This quick start demonstrates how toocreate an Azure Cosmos DB account, document database, and collection using hello Azure portal.</span></span> <span data-ttu-id="d3fbe-107">Potom sestavení a spuštění konzoly aplikace založená na hello [DocumentDB Node.js API](documentdb-sdk-node.md).</span><span class="sxs-lookup"><span data-stu-id="d3fbe-107">You then build and run a console app built on hello [DocumentDB Node.js API](documentdb-sdk-node.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d3fbe-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d3fbe-108">Prerequisites</span></span>

* <span data-ttu-id="d3fbe-109">Před spuštěním této ukázce, musíte mít hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="d3fbe-109">Before you can run this sample, you must have hello following prerequisites:</span></span>
    * <span data-ttu-id="d3fbe-110">[Node.js](https://nodejs.org/en/) verze 0.10.29 nebo vyšší</span><span class="sxs-lookup"><span data-stu-id="d3fbe-110">[Node.js](https://nodejs.org/en/) version v0.10.29 or higher</span></span>
    * [<span data-ttu-id="d3fbe-111">Git</span><span class="sxs-lookup"><span data-stu-id="d3fbe-111">Git</span></span>](http://git-scm.com/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="d3fbe-112">Vytvoření účtu databáze</span><span class="sxs-lookup"><span data-stu-id="d3fbe-112">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a><span data-ttu-id="d3fbe-113">Přidání kolekce</span><span class="sxs-lookup"><span data-stu-id="d3fbe-113">Add a collection</span></span>

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="clone-hello-sample-application"></a><span data-ttu-id="d3fbe-114">Klonování hello ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="d3fbe-114">Clone hello sample application</span></span>

<span data-ttu-id="d3fbe-115">Teď umožňuje nastavit připojovací řetězec hello klonování DocumentDB API aplikace z githubu a potom ho spusťte.</span><span class="sxs-lookup"><span data-stu-id="d3fbe-115">Now let's clone a DocumentDB API app from github, set hello connection string, and run it.</span></span> <span data-ttu-id="d3fbe-116">Uvidíte, jak je snadné toowork s daty prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="d3fbe-116">You see how easy it is toowork with data programmatically.</span></span> 

1. <span data-ttu-id="d3fbe-117">Otevřete okno terminálu git, jako je například git bash a `CD` tooa pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="d3fbe-117">Open a git terminal window, such as git bash, and `CD` tooa working directory.</span></span>  

2. <span data-ttu-id="d3fbe-118">Spusťte následující příkaz tooclone hello Ukázka úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="d3fbe-118">Run hello following command tooclone hello sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-nodejs-getting-started.git
    ```

## <a name="review-hello-code"></a><span data-ttu-id="d3fbe-119">Zkontrolujte hello kódu</span><span class="sxs-lookup"><span data-stu-id="d3fbe-119">Review hello code</span></span>

<span data-ttu-id="d3fbe-120">Provedeme jejich stručný přehled o dění v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="d3fbe-120">Let's make a quick review of what's happening in hello app.</span></span> <span data-ttu-id="d3fbe-121">Otevřete hello `app.js` souboru a najít, že tyto řádky kódu vytvořit hello prostředky Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="d3fbe-121">Open hello `app.js` file and you find that these lines of code create hello Azure Cosmos DB resources.</span></span> 

* <span data-ttu-id="d3fbe-122">Hello `documentClient` je inicializován.</span><span class="sxs-lookup"><span data-stu-id="d3fbe-122">hello `documentClient` is initialized.</span></span>

    ```nodejs
    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });
    ```

* <span data-ttu-id="d3fbe-123">Vytvoří se nová databáze.</span><span class="sxs-lookup"><span data-stu-id="d3fbe-123">A new database is created.</span></span>

    ```nodejs
    client.createDatabase(config.database, (err, created) => {
        if (err) reject(err)
        else resolve(created);
    });
    ```

* <span data-ttu-id="d3fbe-124">Vytvoří se nová kolekce.</span><span class="sxs-lookup"><span data-stu-id="d3fbe-124">A new collection is created.</span></span>

    ```nodejs
    client.createCollection(databaseUrl, config.collection, { offerThroughput: 400 }, (err, created) => {
        if (err) reject(err)
        else resolve(created);
    });
    ```

* <span data-ttu-id="d3fbe-125">Vytvoří se některé dokumenty.</span><span class="sxs-lookup"><span data-stu-id="d3fbe-125">Some documents are created.</span></span>

    ```nodejs
    client.createDocument(collectionUrl, document, (err, created) => {
        if (err) reject(err)
        else resolve(created);
    });
    ```

* <span data-ttu-id="d3fbe-126">Provede se dotaz SQL přes JSON.</span><span class="sxs-lookup"><span data-stu-id="d3fbe-126">A SQL query over JSON is performed.</span></span>

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

## <a name="update-your-connection-string"></a><span data-ttu-id="d3fbe-127">Aktualizace připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="d3fbe-127">Update your connection string</span></span>

<span data-ttu-id="d3fbe-128">Nyní přejděte zpět toohello Azure portálu tooget vaše informace o připojovacím řetězci a zkopírujte jej do aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="d3fbe-128">Now go back toohello Azure portal tooget your connection string information and copy it into hello app.</span></span>

1. <span data-ttu-id="d3fbe-129">V hello [portál Azure](http://portal.azure.com/), ve vašem Azure Cosmos DB účet, klikněte v levé navigační hello **klíče**a potom klikněte na **klíče pro čtení a zápis**.</span><span class="sxs-lookup"><span data-stu-id="d3fbe-129">In hello [Azure portal](http://portal.azure.com/), in your Azure Cosmos DB account, in hello left navigation click **Keys**, and then click **Read-write Keys**.</span></span> <span data-ttu-id="d3fbe-130">Použijete tlačítka hello kopírovat na pravé straně hello hello obrazovky toocopy hello URI a Primary Key do hello `config.js` souboru v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="d3fbe-130">You'll use hello copy buttons on hello right side of hello screen toocopy hello URI and Primary Key into hello `config.js` file in hello next step.</span></span>

    ![Zobrazení a zkopírování přístupový klíč v hello portálu Azure, okna klíče](./media/create-documentdb-dotnet/keys.png)

2. <span data-ttu-id="d3fbe-132">V otevřené hello `config.js` souboru.</span><span class="sxs-lookup"><span data-stu-id="d3fbe-132">In Open hello `config.js` file.</span></span> 

3. <span data-ttu-id="d3fbe-133">Zkopírujte URI hodnota z portálu hello (pomocí tlačítka kopírování hello) a nastavit jej jako hello hodnotu klíče hello koncového bodu v `config.js`.</span><span class="sxs-lookup"><span data-stu-id="d3fbe-133">Copy your URI value from hello portal (using hello copy button) and make it hello value of hello endpoint key in `config.js`.</span></span> 

    `config.endpoint = "https://FILLME.documents.azure.com"`

4. <span data-ttu-id="d3fbe-134">Poté zkopírujte primární klíč hodnota z portálu hello a nastavit jej jako hello hodnotu hello `config.primaryKey` v `config.js`.</span><span class="sxs-lookup"><span data-stu-id="d3fbe-134">Then copy your PRIMARY KEY value from hello portal and make it hello value of hello `config.primaryKey` in `config.js`.</span></span> <span data-ttu-id="d3fbe-135">Jste nyní aktualizovat vaši aplikaci s všechny údaje hello potřebuje toocommunicate s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="d3fbe-135">You've now updated your app with all hello info it needs toocommunicate with Azure Cosmos DB.</span></span> 

    `config.primaryKey "FILLME"`
    
## <a name="run-hello-app"></a><span data-ttu-id="d3fbe-136">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="d3fbe-136">Run hello app</span></span>
1. <span data-ttu-id="d3fbe-137">Spustit `npm install` v terminálu tooinstall požadované moduly npm</span><span class="sxs-lookup"><span data-stu-id="d3fbe-137">Run `npm install` in a terminal tooinstall required npm modules</span></span>

2. <span data-ttu-id="d3fbe-138">Spustit `node app.js` v terminálu toostart aplikace uzlu.</span><span class="sxs-lookup"><span data-stu-id="d3fbe-138">Run `node app.js` in a terminal toostart your node application.</span></span>

<span data-ttu-id="d3fbe-139">Teď můžete přejít zpět tooData Průzkumníka a zobrazit dotaz, upravit a pracovat s Tato nová data.</span><span class="sxs-lookup"><span data-stu-id="d3fbe-139">You can now go back tooData Explorer and see query, modify, and work with this new data.</span></span> 

## <a name="review-slas-in-hello-azure-portal"></a><span data-ttu-id="d3fbe-140">Zkontrolujte SLA v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="d3fbe-140">Review SLAs in hello Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="d3fbe-141">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="d3fbe-141">Clean up resources</span></span>

<span data-ttu-id="d3fbe-142">Pokud ale nebudete toocontinue toouse této aplikace, odstraňte všechny prostředky, které jsou vytvořené tento rychlý start v hello portál Azure s hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d3fbe-142">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span>

1. <span data-ttu-id="d3fbe-143">V levé nabídce hello v hello portálu Azure klikněte na **skupiny prostředků** a pak klikněte na název hello hello prostředků, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="d3fbe-143">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="d3fbe-144">Na stránce skupiny prostředků, klikněte na tlačítko **odstranit**hello textového pole zadejte název hello toodelete hello prostředků a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="d3fbe-144">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d3fbe-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d3fbe-145">Next steps</span></span>

<span data-ttu-id="d3fbe-146">V tento rychlý start když jste se naučili jak toocreate účtu Azure Cosmos DB, vytvořte kolekci pomocí hello Průzkumníku dat a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d3fbe-146">In this quickstart, you've learned how toocreate an Azure Cosmos DB account, create a collection using hello Data Explorer, and run an app.</span></span> <span data-ttu-id="d3fbe-147">Nyní můžete importovat další data tooyour Cosmos DB účtu.</span><span class="sxs-lookup"><span data-stu-id="d3fbe-147">You can now import additional data tooyour Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="d3fbe-148">Importování dat do služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="d3fbe-148">Import data into Azure Cosmos DB</span></span>](import-data.md)


