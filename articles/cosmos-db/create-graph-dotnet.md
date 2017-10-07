---
title: "hello aaaBuild aplikace Azure Cosmos DB .NET pomocí rozhraní Graph API | Microsoft Docs"
description: "Uvede ukázku kódu rozhraní .NET, můžete použít tooconnect tooand dotaz na databázi Azure Cosmos"
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
ms.date: 07/28/2017
ms.author: denlee
ms.openlocfilehash: f28790fcb8c9f57c7bb3d844d8276fa04abcc39c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-net-application-using-hello-graph-api"></a><span data-ttu-id="2f219-103">Azure Cosmos DB: Vytvoření aplikace .NET pomocí hello rozhraní Graph API</span><span class="sxs-lookup"><span data-stu-id="2f219-103">Azure Cosmos DB: Build a .NET application using hello Graph API</span></span>

<span data-ttu-id="2f219-104">Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku.</span><span class="sxs-lookup"><span data-stu-id="2f219-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="2f219-105">Můžete rychle vytvořit a dotazovat dokumentu, klíč/hodnota a graf databází, které těžit z globální distribuční hello a možnosti vodorovné škálování jádrem hello Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2f219-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="2f219-106">Tento rychlý start předvádí, jak hello toocreate účet Azure Cosmos DB, databáze a graf (kontejner) pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2f219-106">This quick start demonstrates how toocreate an Azure Cosmos DB account, database, and graph (container) using hello Azure portal.</span></span> <span data-ttu-id="2f219-107">Potom sestavení a spuštění konzoly aplikace založená na hello [rozhraní Graph API](graph-sdk-dotnet.md) (preview).</span><span class="sxs-lookup"><span data-stu-id="2f219-107">You then build and run a console app built on hello [Graph API](graph-sdk-dotnet.md) (preview).</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="2f219-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2f219-108">Prerequisites</span></span>

<span data-ttu-id="2f219-109">Pokud ještě nemáte nainstalované Visual Studio 2017, můžete stáhnout a použít hello **volné** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="2f219-109">If you don’t already have Visual Studio 2017 installed, you can download and use hello **free** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="2f219-110">Ujistěte se, že povolíte **Azure development** při instalaci sady Visual Studio hello.</span><span class="sxs-lookup"><span data-stu-id="2f219-110">Make sure that you enable **Azure development** during hello Visual Studio setup.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="2f219-111">Vytvoření účtu databáze</span><span class="sxs-lookup"><span data-stu-id="2f219-111">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a><span data-ttu-id="2f219-112">Přidání grafu</span><span class="sxs-lookup"><span data-stu-id="2f219-112">Add a graph</span></span>

[!INCLUDE [cosmos-db-create-graph](../../includes/cosmos-db-create-graph.md)]

## <a name="clone-hello-sample-application"></a><span data-ttu-id="2f219-113">Klonování hello ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="2f219-113">Clone hello sample application</span></span>

<span data-ttu-id="2f219-114">Teď umožňuje nastavit připojovací řetězec hello klonování rozhraní Graph API aplikace z githubu a potom ho spusťte.</span><span class="sxs-lookup"><span data-stu-id="2f219-114">Now let's clone a Graph API app from github, set hello connection string, and run it.</span></span> <span data-ttu-id="2f219-115">Uvidíte, jak je snadné toowork s daty prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="2f219-115">You'll see how easy it is toowork with data programmatically.</span></span> 

1. <span data-ttu-id="2f219-116">Otevřete okno terminálu git, jako je například git bash a `cd` tooa pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="2f219-116">Open a git terminal window, such as git bash, and `cd` tooa working directory.</span></span>  

2. <span data-ttu-id="2f219-117">Spusťte následující příkaz tooclone hello Ukázka úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="2f219-117">Run hello following command tooclone hello sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-dotnet-getting-started.git
    ```

3. <span data-ttu-id="2f219-118">Poté otevřete Visual Studio a hello otevřete soubor řešení.</span><span class="sxs-lookup"><span data-stu-id="2f219-118">Then open Visual Studio and open hello solution file.</span></span> 

## <a name="review-hello-code"></a><span data-ttu-id="2f219-119">Zkontrolujte hello kódu</span><span class="sxs-lookup"><span data-stu-id="2f219-119">Review hello code</span></span>

<span data-ttu-id="2f219-120">Provedeme jejich stručný přehled o dění v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="2f219-120">Let's make a quick review of what's happening in hello app.</span></span> <span data-ttu-id="2f219-121">Hello otevřete soubor Program.cs a najdete, že tyto řádky kódu vytvořit hello prostředky Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2f219-121">Open hello Program.cs file and you'll find that these lines of code create hello Azure Cosmos DB resources.</span></span> 

* <span data-ttu-id="2f219-122">Hello DocumentClient je inicializován.</span><span class="sxs-lookup"><span data-stu-id="2f219-122">hello DocumentClient is initialized.</span></span> <span data-ttu-id="2f219-123">Ve verzi preview hello jsme přidali grafu rozšíření rozhraní API v klientovi Azure Cosmos DB hello.</span><span class="sxs-lookup"><span data-stu-id="2f219-123">In hello preview, we added a graph extension API on hello Azure Cosmos DB client.</span></span> <span data-ttu-id="2f219-124">Pracujeme na klientovi samostatný graf odpojená od klienta Azure Cosmos DB hello a prostředky.</span><span class="sxs-lookup"><span data-stu-id="2f219-124">We are working on a standalone graph client decoupled from hello Azure Cosmos DB client and resources.</span></span>

    ```csharp
    using (DocumentClient client = new DocumentClient(
        new Uri(endpoint),
        authKey,
        new ConnectionPolicy { ConnectionMode = ConnectionMode.Direct, ConnectionProtocol = Protocol.Tcp }))
    ```

* <span data-ttu-id="2f219-125">Vytvoří se nová databáze.</span><span class="sxs-lookup"><span data-stu-id="2f219-125">A new database is created.</span></span>

    ```csharp
    Database database = await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "graphdb" });
    ```

* <span data-ttu-id="2f219-126">Vytvoří se nový graf.</span><span class="sxs-lookup"><span data-stu-id="2f219-126">A new graph is created.</span></span>

    ```csharp
    DocumentCollection graph = await client.CreateDocumentCollectionIfNotExistsAsync(
        UriFactory.CreateDatabaseUri("graphdb"),
        new DocumentCollection { Id = "graph" },
        new RequestOptions { OfferThroughput = 1000 });
    ```
* <span data-ttu-id="2f219-127">Sérii kroků Gremlin jsou spouštěny pomocí hello `CreateGremlinQuery` metoda.</span><span class="sxs-lookup"><span data-stu-id="2f219-127">A series of Gremlin steps are executed using hello `CreateGremlinQuery` method.</span></span>

    ```csharp
    // hello CreateGremlinQuery method extensions allow you tooexecute Gremlin queries and iterate
    // results asychronously
    IDocumentQuery<dynamic> query = client.CreateGremlinQuery<dynamic>(graph, "g.V().count()");
    while (query.HasMoreResults)
    {
        foreach (dynamic result in await query.ExecuteNextAsync())
        {
            Console.WriteLine($"\t {JsonConvert.SerializeObject(result)}");
        }
    }

    ```

## <a name="update-your-connection-string"></a><span data-ttu-id="2f219-128">Aktualizace připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="2f219-128">Update your connection string</span></span>

<span data-ttu-id="2f219-129">Nyní přejděte zpět toohello Azure portálu tooget vaše informace o připojovacím řetězci a zkopírujte jej do aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="2f219-129">Now go back toohello Azure portal tooget your connection string information and copy it into hello app.</span></span>

1. <span data-ttu-id="2f219-130">V aplikaci Visual Studio 2017 otevřete soubor App.config hello.</span><span class="sxs-lookup"><span data-stu-id="2f219-130">In Visual Studio 2017, open hello App.config file.</span></span> 

2. <span data-ttu-id="2f219-131">V hello portál Azure, v účtu Azure Cosmos DB, klikněte na **klíče** v levé navigační hello.</span><span class="sxs-lookup"><span data-stu-id="2f219-131">In hello Azure portal, in your Azure Cosmos DB account, click **Keys** in hello left navigation.</span></span> 

    ![Zobrazení a zkopírujte primární klíč v hello portál Azure, na stránce klíče hello](./media/create-graph-dotnet/keys.png)

3. <span data-ttu-id="2f219-133">Kopie vašeho **URI** hodnoty z portálu hello a nastavit jej jako hello hodnotu klíče hello koncový bod v souboru App.config. Můžete použít tlačítko Kopírovat hello, jak je uvedeno v předchozím snímku obrazovky toocopy hello hodnota hello.</span><span class="sxs-lookup"><span data-stu-id="2f219-133">Copy your **URI** value from hello portal and make it hello value of hello Endpoint key in App.config. You can use hello copy button as shown in hello preceding screenshot toocopy hello value.</span></span>

    `<add key="Endpoint" value="https://FILLME.documents.azure.com:443" />`

4. <span data-ttu-id="2f219-134">Kopie vašeho **primární klíč** hodnoty z portálu hello a nastavit jej jako hello hodnota hello AuthKey klíče v souboru App.config a potom uložte změny.</span><span class="sxs-lookup"><span data-stu-id="2f219-134">Copy your **PRIMARY KEY** value from hello portal, and make it hello value of hello AuthKey key in App.config, then save your changes.</span></span> 

    `<add key="AuthKey" value="FILLME" />`

<span data-ttu-id="2f219-135">Jste nyní aktualizovat vaši aplikaci s všechny údaje hello potřebuje toocommunicate s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2f219-135">You've now updated your app with all hello info it needs toocommunicate with Azure Cosmos DB.</span></span> 

## <a name="run-hello-console-app"></a><span data-ttu-id="2f219-136">Spusťte konzolovou aplikaci hello</span><span class="sxs-lookup"><span data-stu-id="2f219-136">Run hello console app</span></span>

1. <span data-ttu-id="2f219-137">V sadě Visual Studio, klikněte pravým tlačítkem na hello **GraphGetStarted** projektu v **Průzkumníku řešení** a pak klikněte na **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="2f219-137">In Visual Studio, right-click on hello **GraphGetStarted** project in **Solution Explorer** and then click **Manage NuGet Packages**.</span></span> 

2. <span data-ttu-id="2f219-138">V hello NuGet **Procházet** zadejte *Microsoft.Azure.Graphs* a zkontrolujte hello **zahrnuje předběžné verze** pole.</span><span class="sxs-lookup"><span data-stu-id="2f219-138">In hello NuGet **Browse** box, type *Microsoft.Azure.Graphs* and check hello **Includes prerelease** box.</span></span> 

3. <span data-ttu-id="2f219-139">Z výsledků hello nainstalovat hello **Microsoft.Azure.Graphs** knihovny.</span><span class="sxs-lookup"><span data-stu-id="2f219-139">From hello results, install hello **Microsoft.Azure.Graphs** library.</span></span> <span data-ttu-id="2f219-140">Tím se nainstaluje balíček knihovny hello Azure Cosmos DB grafu rozšíření a všechny závislosti.</span><span class="sxs-lookup"><span data-stu-id="2f219-140">This installs hello Azure Cosmos DB graph extension library package and all dependencies.</span></span>

    <span data-ttu-id="2f219-141">Pokud se zobrazí zpráva o Kontrola řešení toohello změny, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="2f219-141">If you get a message about reviewing changes toohello solution, click **OK**.</span></span> <span data-ttu-id="2f219-142">Pokud se vám zobrazí zpráva týkající se přijetí licence, klikněte na **Souhlasím**.</span><span class="sxs-lookup"><span data-stu-id="2f219-142">If you get a message about license acceptance, click **I accept**.</span></span>

4. <span data-ttu-id="2f219-143">Klikněte na kombinaci kláves CTRL + F5 toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="2f219-143">Click CTRL + F5 toorun hello application.</span></span>

   <span data-ttu-id="2f219-144">okno konzoly Hello zobrazí bodů uchycení hello a okrajů přidávané toohello grafu.</span><span class="sxs-lookup"><span data-stu-id="2f219-144">hello console window displays hello vertexes and edges being added toohello graph.</span></span> <span data-ttu-id="2f219-145">Když hello dokončení skriptu, stiskněte klávesu ENTER dvakrát tooclose okna konzoly hello.</span><span class="sxs-lookup"><span data-stu-id="2f219-145">When hello script completes, press ENTER twice tooclose hello console window.</span></span> 

## <a name="browse-using-hello-data-explorer"></a><span data-ttu-id="2f219-146">Procházet pomocí hello Průzkumníku dat</span><span class="sxs-lookup"><span data-stu-id="2f219-146">Browse using hello Data Explorer</span></span>

<span data-ttu-id="2f219-147">Teď můžete přejít zpět tooData Explorer v hello portál Azure a procházet a dotaz na nová data grafu.</span><span class="sxs-lookup"><span data-stu-id="2f219-147">You can now go back tooData Explorer in hello Azure portal and browse and query your new graph data.</span></span>

1. <span data-ttu-id="2f219-148">V Průzkumníku dat hello nové databáze se zobrazí v podokně grafy hello.</span><span class="sxs-lookup"><span data-stu-id="2f219-148">In Data Explorer, hello new database appears in hello Graphs pane.</span></span> <span data-ttu-id="2f219-149">Rozbalte položky **graphdb** a **graphcollz** a potom klikněte na **Graph**.</span><span class="sxs-lookup"><span data-stu-id="2f219-149">Expand **graphdb**, **graphcollz**, and then click **Graph**.</span></span>

2. <span data-ttu-id="2f219-150">Klikněte na tlačítko hello **použít filtr** tlačítko toouse hello výchozí dotaz tooview všechny verticies hello v grafu hello.</span><span class="sxs-lookup"><span data-stu-id="2f219-150">Click hello **Apply Filter** button toouse hello default query tooview all hello verticies in hello graph.</span></span> <span data-ttu-id="2f219-151">Hello data generována hello ukázkové aplikace se zobrazí v podokně grafy hello.</span><span class="sxs-lookup"><span data-stu-id="2f219-151">hello data generated by hello sample app is displayed in hello Graphs pane.</span></span>

    <span data-ttu-id="2f219-152">Oddálit hello grafu, můžete zvětšit místo zobrazení grafu hello, přidat další verticies a přesunout verticies na hello zobrazit prostor.</span><span class="sxs-lookup"><span data-stu-id="2f219-152">You can zoom in and out of hello graph, you can expand hello graph display space, add additional verticies, and move verticies on hello display surface.</span></span>

    ![Zobrazit graf hello v Průzkumníku dat v hello portálu Azure](./media/create-graph-dotnet/graph-explorer.png)

## <a name="review-slas-in-hello-azure-portal"></a><span data-ttu-id="2f219-154">Zkontrolujte SLA v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="2f219-154">Review SLAs in hello Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="2f219-155">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="2f219-155">Clean up resources</span></span>

<span data-ttu-id="2f219-156">Pokud ale nebudete toocontinue toouse této aplikace, odstraňte všechny prostředky, které jsou vytvořené tento rychlý start v hello portál Azure s hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2f219-156">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span> 

1. <span data-ttu-id="2f219-157">V levé nabídce hello v hello portálu Azure klikněte na **skupiny prostředků** a pak klikněte na název hello hello prostředků, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="2f219-157">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="2f219-158">Na stránce skupiny prostředků, klikněte na tlačítko **odstranit**hello textového pole zadejte název hello toodelete hello prostředků a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="2f219-158">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2f219-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2f219-159">Next steps</span></span>

<span data-ttu-id="2f219-160">V tento rychlý start když jste se naučili toocreate účet Azure Cosmos DB vytvoření grafu pomocí hello Průzkumníku dat a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2f219-160">In this quickstart, you've learned how toocreate an Azure Cosmos DB account, create a graph using hello Data Explorer, and run an app.</span></span> <span data-ttu-id="2f219-161">Teď můžete pomocí konzoly Gremlin vytvářet složitější dotazy a implementovat účinnou logiku procházení grafů.</span><span class="sxs-lookup"><span data-stu-id="2f219-161">You can now build more complex queries and implement powerful graph traversal logic using Gremlin.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="2f219-162">Dotazování pomocí konzoly Gremlin</span><span class="sxs-lookup"><span data-stu-id="2f219-162">Query using Gremlin</span></span>](tutorial-query-graph.md)

