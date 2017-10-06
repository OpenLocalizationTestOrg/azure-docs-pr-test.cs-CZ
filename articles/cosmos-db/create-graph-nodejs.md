---
title: "aaaBuild aplikace Azure Cosmos DB Node.js pomocí rozhraní Graph API | Microsoft Docs"
description: "Uvede ukázku kódu Node.js můžete použít tooconnect tooand dotaz na databázi Azure Cosmos"
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
ms.assetid: daacbabf-1bb5-497f-92db-079910703046
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 07/14/2017
ms.author: denlee
ms.openlocfilehash: 1445755842bc4e4a84ca2b2f789aadde8467e190
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-nodejs-application-by-using-graph-api"></a><span data-ttu-id="af3e6-103">Databáze Azure Cosmos: Vytvoření aplikace Node.js využitím rozhraní Graph API</span><span class="sxs-lookup"><span data-stu-id="af3e6-103">Azure Cosmos DB: Build a Node.js application by using Graph API</span></span>

<span data-ttu-id="af3e6-104">Azure Cosmos DB je služba více modelu databáze hello globálně distribuované od společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="af3e6-104">Azure Cosmos DB is hello globally distributed multi-model database service from Microsoft.</span></span> <span data-ttu-id="af3e6-105">Můžete rychle vytvořit a dotazovat dokumentu, klíč/hodnota a graf databází, které těžit z globální distribuční hello a možnosti vodorovné škálování jádrem hello Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="af3e6-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="af3e6-106">Tento úvodní článek ukazuje, jak toocreate Azure DB Cosmos účet pro rozhraní Graph API (preview), databáze a grafu pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="af3e6-106">This quick-start article demonstrates how toocreate an Azure Cosmos DB account for Graph API (preview), database, and graph by using hello Azure portal.</span></span> <span data-ttu-id="af3e6-107">Potom sestavit a spustit aplikaci konzoly pomocí hello open-source [Gremlin Node.js](https://www.npmjs.com/package/gremlin-secure) ovladačů.</span><span class="sxs-lookup"><span data-stu-id="af3e6-107">You then build and run a console app by using hello open-source [Gremlin Node.js](https://www.npmjs.com/package/gremlin-secure) driver.</span></span>  

> [!NOTE]
> <span data-ttu-id="af3e6-108">modul npm Hello `gremlin-secure` je upravenou verzi `gremlin` modul, se podpora pro protokol SSL a SASL požadované pro připojení k databázi Azure Cosmos.</span><span class="sxs-lookup"><span data-stu-id="af3e6-108">hello npm module `gremlin-secure` is a modified version of `gremlin` module, with support for SSL and SASL required for connecting with Azure Cosmos DB.</span></span> <span data-ttu-id="af3e6-109">Zdrojový kód je k dispozici na [GitHubu](https://github.com/CosmosDB/gremlin-javascript).</span><span class="sxs-lookup"><span data-stu-id="af3e6-109">Source code is available on [GitHub](https://github.com/CosmosDB/gremlin-javascript).</span></span>
>

## <a name="prerequisites"></a><span data-ttu-id="af3e6-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="af3e6-110">Prerequisites</span></span>

<span data-ttu-id="af3e6-111">Před spuštěním této ukázce, musíte mít hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="af3e6-111">Before you can run this sample, you must have hello following prerequisites:</span></span>
* <span data-ttu-id="af3e6-112">[Node.js](https://nodejs.org/en/) verze 0.10.29 nebo novější</span><span class="sxs-lookup"><span data-stu-id="af3e6-112">[Node.js](https://nodejs.org/en/) version v0.10.29 or later</span></span>
* [<span data-ttu-id="af3e6-113">Git</span><span class="sxs-lookup"><span data-stu-id="af3e6-113">Git</span></span>](http://git-scm.com/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="af3e6-114">Vytvoření účtu databáze</span><span class="sxs-lookup"><span data-stu-id="af3e6-114">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a><span data-ttu-id="af3e6-115">Přidání grafu</span><span class="sxs-lookup"><span data-stu-id="af3e6-115">Add a graph</span></span>

[!INCLUDE [cosmos-db-create-graph](../../includes/cosmos-db-create-graph.md)]

## <a name="clone-hello-sample-application"></a><span data-ttu-id="af3e6-116">Klonování hello ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="af3e6-116">Clone hello sample application</span></span>

<span data-ttu-id="af3e6-117">Teď umožňuje nastavit připojovací řetězec hello klonování rozhraní Graph API aplikace z Githubu a potom ho spusťte.</span><span class="sxs-lookup"><span data-stu-id="af3e6-117">Now let's clone a Graph API app from GitHub, set hello connection string, and run it.</span></span> <span data-ttu-id="af3e6-118">Uvidíte, jak je snadné toowork s daty prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="af3e6-118">You'll see how easy it is toowork with data programmatically.</span></span> 

1. <span data-ttu-id="af3e6-119">Otevřete okno terminálu Git, jako je například Git Bash a změňte (prostřednictvím `cd` příkaz) tooa pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="af3e6-119">Open a Git terminal window, such as Git Bash, and change (via `cd` command) tooa working directory.</span></span>  

2. <span data-ttu-id="af3e6-120">Spusťte následující příkaz tooclone hello Ukázka úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="af3e6-120">Run hello following command tooclone hello sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-nodejs-getting-started.git
    ```

3. <span data-ttu-id="af3e6-121">Otevřete soubor řešení hello v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="af3e6-121">Open hello solution file in Visual Studio.</span></span> 

## <a name="review-hello-code"></a><span data-ttu-id="af3e6-122">Zkontrolujte hello kódu</span><span class="sxs-lookup"><span data-stu-id="af3e6-122">Review hello code</span></span>

<span data-ttu-id="af3e6-123">Provedeme jejich stručný přehled o dění v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="af3e6-123">Let's make a quick review of what's happening in hello app.</span></span> <span data-ttu-id="af3e6-124">Otevřete hello `app.js` soubor a najdete hello následující řádky kódu.</span><span class="sxs-lookup"><span data-stu-id="af3e6-124">Open hello `app.js` file, and you'll find hello following lines of code.</span></span> 

* <span data-ttu-id="af3e6-125">Vytvoří se klient Gremlin Hello.</span><span class="sxs-lookup"><span data-stu-id="af3e6-125">hello Gremlin client is created.</span></span>

    ```nodejs
    const client = Gremlin.createClient(
        443, 
        config.endpoint, 
        { 
            "session": false, 
            "ssl": true, 
            "user": `/dbs/${config.database}/colls/${config.collection}`,
            "password": config.primaryKey
        });
    ```

  <span data-ttu-id="af3e6-126">Hello konfigurace jsou v `config.js`, který jsme upravit v následující části hello.</span><span class="sxs-lookup"><span data-stu-id="af3e6-126">hello configurations are all in `config.js`, which we edit in hello following section.</span></span>

* <span data-ttu-id="af3e6-127">Sérii kroků Gremlin jsou spouštěny s hello `client.execute` metoda.</span><span class="sxs-lookup"><span data-stu-id="af3e6-127">A series of Gremlin steps are executed with hello `client.execute` method.</span></span>

    ```nodejs
    console.log('Running Count'); 
    client.execute("g.V().count()", { }, (err, results) => {
        if (err) return console.error(err);
        console.log(JSON.stringify(results));
        console.log();
    });
    ```

## <a name="update-your-connection-string"></a><span data-ttu-id="af3e6-128">Aktualizace připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="af3e6-128">Update your connection string</span></span>

1. <span data-ttu-id="af3e6-129">Otevřete hello souboru config.js.</span><span class="sxs-lookup"><span data-stu-id="af3e6-129">Open hello config.js file.</span></span> 

2. <span data-ttu-id="af3e6-130">V souboru config.js, vyplňte hello config.endpoint klíč s hello **Gremlin URI** hodnotu z hello **přehled** stránku hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="af3e6-130">In config.js, fill in hello config.endpoint key with hello **Gremlin URI** value from hello **Overview** page of hello Azure portal.</span></span> 

    `config.endpoint = "GRAPHENDPOINT";`

    ![Zobrazení a zkopírování přístupový klíč v hello portálu Azure, okna klíče](./media/create-graph-nodejs/gremlin-uri.png)

   <span data-ttu-id="af3e6-132">Pokud hello **Gremlin URI** je hodnota prázdná, můžete z hello vygenerovat hello hodnotu **klíče** stránku hello portálu pomocí hello **URI** hodnotu, odebrání https:// a změna toographs dokumenty.</span><span class="sxs-lookup"><span data-stu-id="af3e6-132">If hello **Gremlin URI** value is blank, you can generate hello value from hello **Keys** page in hello portal, using hello **URI** value, removing https://, and changing documents toographs.</span></span>

   <span data-ttu-id="af3e6-133">Hello Gremlin koncový bod musí být jen název hostitele hello bez hello čísla portu nebo protokolu, jako je třeba `mygraphdb.graphs.azure.com` (ne `https://mygraphdb.graphs.azure.com` nebo `mygraphdb.graphs.azure.com:433`).</span><span class="sxs-lookup"><span data-stu-id="af3e6-133">hello Gremlin endpoint must be only hello host name without hello protocol/port number, like `mygraphdb.graphs.azure.com` (not `https://mygraphdb.graphs.azure.com` or `mygraphdb.graphs.azure.com:433`).</span></span>

3. <span data-ttu-id="af3e6-134">V souboru config.js, zadejte hodnotu config.primaryKey hello pomocí hello **primární klíč** hodnotu z hello **klíče** stránku hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="af3e6-134">In config.js, fill in hello config.primaryKey value in with hello **Primary Key** value from hello **Keys** page of hello Azure portal.</span></span> 

    `config.primaryKey = "PRIMARYKEY";`

   ![Hello Azure portálu okna klíče](./media/create-graph-nodejs/keys.png)

4. <span data-ttu-id="af3e6-136">Zadejte název databáze hello a název grafu (kontejner) pro hodnotu hello config.database a config.collection.</span><span class="sxs-lookup"><span data-stu-id="af3e6-136">Enter hello database name, and graph (container) name for hello value of config.database and config.collection.</span></span> 

<span data-ttu-id="af3e6-137">Tady je příklad, jak by dokončený soubor config.js měl vypadat:</span><span class="sxs-lookup"><span data-stu-id="af3e6-137">Here is an example of what your completed config.js file should look like:</span></span>

```nodejs
var config = {}

// Note that this must not have HTTPS or hello port number
config.endpoint = "testgraphacct.graphs.azure.com";
config.primaryKey = "Pams6e7LEUS7LJ2Qk0fjZf3eGo65JdMWHmyn65i52w8ozPX2oxY3iP0yu05t9v1WymAHNcMwPIqNAEv3XDFsEg==";
config.database = "graphdb"
config.collection = "Persons"

module.exports = config;
```

## <a name="run-hello-console-app"></a><span data-ttu-id="af3e6-138">Spusťte konzolovou aplikaci hello</span><span class="sxs-lookup"><span data-stu-id="af3e6-138">Run hello console app</span></span>

1. <span data-ttu-id="af3e6-139">Otevřete okno terminálu a změňte (prostřednictvím `cd` příkaz) toohello instalační adresář pro soubor package.json hello, který je součástí projektu hello.</span><span class="sxs-lookup"><span data-stu-id="af3e6-139">Open a terminal window and change (via `cd` command) toohello installation directory for hello package.json file that's included in hello project.</span></span>  

2. <span data-ttu-id="af3e6-140">Spustit `npm install` tooinstall hello požadované moduly npm, včetně `gremlin-secure`.</span><span class="sxs-lookup"><span data-stu-id="af3e6-140">Run `npm install` tooinstall hello required npm modules, including `gremlin-secure`.</span></span>

3. <span data-ttu-id="af3e6-141">Spustit `node app.js` v terminálu toostart aplikace uzlu.</span><span class="sxs-lookup"><span data-stu-id="af3e6-141">Run `node app.js` in a terminal toostart your node application.</span></span>

## <a name="browse-with-data-explorer"></a><span data-ttu-id="af3e6-142">Procházení pomocí Průzkumníku dat</span><span class="sxs-lookup"><span data-stu-id="af3e6-142">Browse with Data Explorer</span></span>

<span data-ttu-id="af3e6-143">Teď můžete vrátit tooData Explorer v hello Azure portálu tooview, dotaz, upravit a pracovat s daty nový graf.</span><span class="sxs-lookup"><span data-stu-id="af3e6-143">You can now go back tooData Explorer in hello Azure portal tooview, query, modify, and work with your new graph data.</span></span>

<span data-ttu-id="af3e6-144">V Průzkumníku dat hello nové databáze se zobrazí v hello **grafy** podokně.</span><span class="sxs-lookup"><span data-stu-id="af3e6-144">In Data Explorer, hello new database appears in hello **Graphs** pane.</span></span> <span data-ttu-id="af3e6-145">Rozbalte databázi hello, za nímž následuje hello kolekce, a poté klikněte na tlačítko **grafu**.</span><span class="sxs-lookup"><span data-stu-id="af3e6-145">Expand hello database, followed by hello collection, then click **Graph**.</span></span>

<span data-ttu-id="af3e6-146">Hello data generována hello ukázkové aplikace se zobrazí v další podokno hello v rámci hello **grafu** kartě po kliknutí na tlačítko **použít filtr**.</span><span class="sxs-lookup"><span data-stu-id="af3e6-146">hello data generated by hello sample app is displayed in hello next pane within hello **Graph** tab when you click **Apply Filter**.</span></span>

<span data-ttu-id="af3e6-147">Zkuste dokončení `g.V()` s `.has('firstName', 'Thomas')` tootest hello filtru.</span><span class="sxs-lookup"><span data-stu-id="af3e6-147">Try completing `g.V()` with `.has('firstName', 'Thomas')` tootest hello filter.</span></span> <span data-ttu-id="af3e6-148">Všimněte si, že hodnota hello je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="af3e6-148">Do note that hello value is case sensitive.</span></span>

## <a name="review-slas-in-hello-azure-portal"></a><span data-ttu-id="af3e6-149">Zkontrolujte SLA v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="af3e6-149">Review SLAs in hello Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-your-resources"></a><span data-ttu-id="af3e6-150">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="af3e6-150">Clean up your resources</span></span>

<span data-ttu-id="af3e6-151">Pokud neplánujete toocontinue používání této aplikace, odstraňte všechny prostředky, které jste vytvořili v tomto článku díky hello následující:</span><span class="sxs-lookup"><span data-stu-id="af3e6-151">If you do not plan toocontinue using this app, delete all resources that you created in this article by doing hello following:</span></span> 

1. <span data-ttu-id="af3e6-152">V hello portál Azure, v nabídce hello navigaci vlevo, klikněte na **skupiny prostředků**a potom klikněte na název hello hello prostředku, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="af3e6-152">In hello Azure portal, on hello left navigation menu, click **Resource groups**, and then click hello name of hello resource that you created.</span></span> 
2. <span data-ttu-id="af3e6-153">Na stránce skupiny prostředků, klikněte na tlačítko **odstranit**, zadejte název hello toobe prostředků hello odstranit a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="af3e6-153">On your resource group page, click **Delete**, type hello name of hello resource toobe deleted, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="af3e6-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="af3e6-154">Next steps</span></span>

<span data-ttu-id="af3e6-155">V tomto článku když jste se naučili toocreate účet Azure Cosmos DB vytvoření grafu pomocí Průzkumníku dat a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="af3e6-155">In this article, you've learned how toocreate an Azure Cosmos DB account, create a graph by using Data Explorer, and run an app.</span></span> <span data-ttu-id="af3e6-156">Teď můžete pomocí konzoly Gremlin vytvářet složitější dotazy a implementovat účinnou logiku procházení grafů.</span><span class="sxs-lookup"><span data-stu-id="af3e6-156">You can now build more complex queries and implement powerful graph traversal logic by using Gremlin.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="af3e6-157">Dotazování pomocí konzoly Gremlin</span><span class="sxs-lookup"><span data-stu-id="af3e6-157">Query using Gremlin</span></span>](tutorial-query-graph.md)
