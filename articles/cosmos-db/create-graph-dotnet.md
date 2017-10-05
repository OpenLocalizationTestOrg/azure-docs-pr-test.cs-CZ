---
title: "Vytvoření aplikace .NET databáze Azure Cosmos využívající rozhraní Graph API | Dokumentace Microsoftu"
description: "Obsahuje ukázku kódu .NET, kterou můžete použít pro připojení a dotazování databáze Azure Cosmos."
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
ms.openlocfilehash: a973b81ea5b06c5826cc31c399aae9dec43f5b72
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-cosmos-db-build-a-net-application-using-the-graph-api"></a><span data-ttu-id="24aee-103">Databáze Azure Cosmos: Vytvoření aplikace .NET využívající rozhraní Graph API</span><span class="sxs-lookup"><span data-stu-id="24aee-103">Azure Cosmos DB: Build a .NET application using the Graph API</span></span>

<span data-ttu-id="24aee-104">Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku.</span><span class="sxs-lookup"><span data-stu-id="24aee-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="24aee-105">Můžete snadno vytvořit a dotazovat databáze dotazů, klíčů/hodnot a grafů, které tak můžou využívat výhody použitelnosti v celosvětovém měřítku a možností horizontálního škálování v jádru databáze Azure Cosmos.</span><span class="sxs-lookup"><span data-stu-id="24aee-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from the global distribution and horizontal scale capabilities at the core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="24aee-106">Tento rychlý start popisuje způsob vytvoření účtu databáze Azure Cosmos, databáze a grafu (kontejneru) pomocí webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="24aee-106">This quick start demonstrates how to create an Azure Cosmos DB account, database, and graph (container) using the Azure portal.</span></span> <span data-ttu-id="24aee-107">Potom sestavíte a spustíte aplikaci konzoly založenou na [rozhraní Graph API](graph-sdk-dotnet.md) (verze Preview).</span><span class="sxs-lookup"><span data-stu-id="24aee-107">You then build and run a console app built on the [Graph API](graph-sdk-dotnet.md) (preview).</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="24aee-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="24aee-108">Prerequisites</span></span>

<span data-ttu-id="24aee-109">Pokud ještě nemáte nainstalovanou sadu Visual Studio 2017, můžete stáhnout a použít **bezplatnou verzi** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="24aee-109">If you don’t already have Visual Studio 2017 installed, you can download and use the **free** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="24aee-110">Nezapomeňte při instalaci sady Visual Studio povolit možnost **Azure Development**.</span><span class="sxs-lookup"><span data-stu-id="24aee-110">Make sure that you enable **Azure development** during the Visual Studio setup.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="24aee-111">Vytvoření účtu databáze</span><span class="sxs-lookup"><span data-stu-id="24aee-111">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a><span data-ttu-id="24aee-112">Přidání grafu</span><span class="sxs-lookup"><span data-stu-id="24aee-112">Add a graph</span></span>

[!INCLUDE [cosmos-db-create-graph](../../includes/cosmos-db-create-graph.md)]

## <a name="clone-the-sample-application"></a><span data-ttu-id="24aee-113">Klonování ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="24aee-113">Clone the sample application</span></span>

<span data-ttu-id="24aee-114">Teď naklonujeme aplikaci rozhraní Graph API z GitHubu, nastavíme připojovací řetězec a spustíme ji.</span><span class="sxs-lookup"><span data-stu-id="24aee-114">Now let's clone a Graph API app from github, set the connection string, and run it.</span></span> <span data-ttu-id="24aee-115">Přesvědčíte se, jak snadno se pracuje s daty prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="24aee-115">You'll see how easy it is to work with data programmatically.</span></span> 

1. <span data-ttu-id="24aee-116">Otevřete okno terminálu Git, jako je třeba Git Bash, a pomocí `cd` přejděte do pracovního adresáře.</span><span class="sxs-lookup"><span data-stu-id="24aee-116">Open a git terminal window, such as git bash, and `cd` to a working directory.</span></span>  

2. <span data-ttu-id="24aee-117">Ukázkové úložiště naklonujete spuštěním následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="24aee-117">Run the following command to clone the sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-dotnet-getting-started.git
    ```

3. <span data-ttu-id="24aee-118">Potom otevřete sadu Visual Studio a otevřete soubor řešení.</span><span class="sxs-lookup"><span data-stu-id="24aee-118">Then open Visual Studio and open the solution file.</span></span> 

## <a name="review-the-code"></a><span data-ttu-id="24aee-119">Kontrola kódu</span><span class="sxs-lookup"><span data-stu-id="24aee-119">Review the code</span></span>

<span data-ttu-id="24aee-120">Ještě jednou se stručně podívejme na to, co se v aplikaci děje.</span><span class="sxs-lookup"><span data-stu-id="24aee-120">Let's make a quick review of what's happening in the app.</span></span> <span data-ttu-id="24aee-121">Otevřete soubor Program.cs a zjistíte, že tyto řádky kódu vytvářejí prostředky databáze Azure Cosmos.</span><span class="sxs-lookup"><span data-stu-id="24aee-121">Open the Program.cs file and you'll find that these lines of code create the Azure Cosmos DB resources.</span></span> 

* <span data-ttu-id="24aee-122">Inicializuje se DocumentClient.</span><span class="sxs-lookup"><span data-stu-id="24aee-122">The DocumentClient is initialized.</span></span> <span data-ttu-id="24aee-123">Ve verzi Preview jsme do klienta Azure Cosmos DB přidali rozhraní API s rozšířením grafu.</span><span class="sxs-lookup"><span data-stu-id="24aee-123">In the preview, we added a graph extension API on the Azure Cosmos DB client.</span></span> <span data-ttu-id="24aee-124">Pracujeme na samostatném klientovi pro grafy, který bude oddělený od klienta a prostředků Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="24aee-124">We are working on a standalone graph client decoupled from the Azure Cosmos DB client and resources.</span></span>

    ```csharp
    using (DocumentClient client = new DocumentClient(
        new Uri(endpoint),
        authKey,
        new ConnectionPolicy { ConnectionMode = ConnectionMode.Direct, ConnectionProtocol = Protocol.Tcp }))
    ```

* <span data-ttu-id="24aee-125">Vytvoří se nová databáze.</span><span class="sxs-lookup"><span data-stu-id="24aee-125">A new database is created.</span></span>

    ```csharp
    Database database = await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "graphdb" });
    ```

* <span data-ttu-id="24aee-126">Vytvoří se nový graf.</span><span class="sxs-lookup"><span data-stu-id="24aee-126">A new graph is created.</span></span>

    ```csharp
    DocumentCollection graph = await client.CreateDocumentCollectionIfNotExistsAsync(
        UriFactory.CreateDatabaseUri("graphdb"),
        new DocumentCollection { Id = "graph" },
        new RequestOptions { OfferThroughput = 1000 });
    ```
* <span data-ttu-id="24aee-127">Pomocí metody `CreateGremlinQuery` se provede série kroků konzoly Gremlin.</span><span class="sxs-lookup"><span data-stu-id="24aee-127">A series of Gremlin steps are executed using the `CreateGremlinQuery` method.</span></span>

    ```csharp
    // The CreateGremlinQuery method extensions allow you to execute Gremlin queries and iterate
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

## <a name="update-your-connection-string"></a><span data-ttu-id="24aee-128">Aktualizace připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="24aee-128">Update your connection string</span></span>

<span data-ttu-id="24aee-129">Teď se vraťte zpátky na portál Azure Portal, kde najdete informace o připojovacím řetězci, a zkopírujte je do aplikace.</span><span class="sxs-lookup"><span data-stu-id="24aee-129">Now go back to the Azure portal to get your connection string information and copy it into the app.</span></span>

1. <span data-ttu-id="24aee-130">V sadě Visual Studio 2017 otevřete soubor App.config.</span><span class="sxs-lookup"><span data-stu-id="24aee-130">In Visual Studio 2017, open the App.config file.</span></span> 

2. <span data-ttu-id="24aee-131">Na webu Azure Portal v účtu služby Azure Cosmos DB klikněte v levém navigačním panelu na **Klíče**.</span><span class="sxs-lookup"><span data-stu-id="24aee-131">In the Azure portal, in your Azure Cosmos DB account, click **Keys** in the left navigation.</span></span> 

    ![Zobrazení a zkopírování primárního klíče na webu Azure Portal na stránce Klíče](./media/create-graph-dotnet/keys.png)

3. <span data-ttu-id="24aee-133">Z portálu zkopírujte hodnotu identifikátoru **URI** a nastavte ji jako hodnotu klíče koncového bodu v souboru App.config.</span><span class="sxs-lookup"><span data-stu-id="24aee-133">Copy your **URI** value from the portal and make it the value of the Endpoint key in App.config.</span></span> <span data-ttu-id="24aee-134">Hodnotu můžete zkopírovat pomocí tlačítka Kopírovat, jak ukazuje předchozí snímek obrazovky.</span><span class="sxs-lookup"><span data-stu-id="24aee-134">You can use the copy button as shown in the preceding screenshot to copy the value.</span></span>

    `<add key="Endpoint" value="https://FILLME.documents.azure.com:443" />`

4. <span data-ttu-id="24aee-135">Z portálu zkopírujte hodnotu **PRIMÁRNÍHO KLÍČE**, nastavte ji jako hodnotu klíče AuthKey v souboru App.config a potom uložte změny.</span><span class="sxs-lookup"><span data-stu-id="24aee-135">Copy your **PRIMARY KEY** value from the portal, and make it the value of the AuthKey key in App.config, then save your changes.</span></span> 

    `<add key="AuthKey" value="FILLME" />`

<span data-ttu-id="24aee-136">Teď jste aktualizovali aplikaci a zadali do ní všechny informace potřebné ke komunikaci s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="24aee-136">You've now updated your app with all the info it needs to communicate with Azure Cosmos DB.</span></span> 

## <a name="run-the-console-app"></a><span data-ttu-id="24aee-137">Spuštění aplikace konzoly</span><span class="sxs-lookup"><span data-stu-id="24aee-137">Run the console app</span></span>

1. <span data-ttu-id="24aee-138">V sadě Visual Studio klikněte v **Průzkumníku řešení** pravým tlačítkem myši na projekt **GraphGetStarted** a potom klikněte na možnost **Spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="24aee-138">In Visual Studio, right-click on the **GraphGetStarted** project in **Solution Explorer** and then click **Manage NuGet Packages**.</span></span> 

2. <span data-ttu-id="24aee-139">Do pole **Procházet** v NuGetu zadejte *WindowsAzure.Graphs* a zaškrtněte políčko **Zahrnout předběžné verze**.</span><span class="sxs-lookup"><span data-stu-id="24aee-139">In the NuGet **Browse** box, type *Microsoft.Azure.Graphs* and check the **Includes prerelease** box.</span></span> 

3. <span data-ttu-id="24aee-140">Z výsledků nainstalujte knihovnu **Microsoft.Azure.Graphs**.</span><span class="sxs-lookup"><span data-stu-id="24aee-140">From the results, install the **Microsoft.Azure.Graphs** library.</span></span> <span data-ttu-id="24aee-141">Tím se nainstaluje balíček knihovny rozšíření grafů databáze Azure Cosmos a všechny závislosti.</span><span class="sxs-lookup"><span data-stu-id="24aee-141">This installs the Azure Cosmos DB graph extension library package and all dependencies.</span></span>

    <span data-ttu-id="24aee-142">Pokud se vám zobrazí zpráva týkající se kontroly změn řešení, klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="24aee-142">If you get a message about reviewing changes to the solution, click **OK**.</span></span> <span data-ttu-id="24aee-143">Pokud se vám zobrazí zpráva týkající se přijetí licence, klikněte na **Souhlasím**.</span><span class="sxs-lookup"><span data-stu-id="24aee-143">If you get a message about license acceptance, click **I accept**.</span></span>

4. <span data-ttu-id="24aee-144">Spusťte aplikaci stisknutím CTRL+F5.</span><span class="sxs-lookup"><span data-stu-id="24aee-144">Click CTRL + F5 to run the application.</span></span>

   <span data-ttu-id="24aee-145">V okně konzoly se zobrazí vrcholy a hrany, které se přidávají do grafu.</span><span class="sxs-lookup"><span data-stu-id="24aee-145">The console window displays the vertexes and edges being added to the graph.</span></span> <span data-ttu-id="24aee-146">Po dokončení skriptu dvojím stisknutím klávesy ENTER zavřete okno konzoly.</span><span class="sxs-lookup"><span data-stu-id="24aee-146">When the script completes, press ENTER twice to close the console window.</span></span> 

## <a name="browse-using-the-data-explorer"></a><span data-ttu-id="24aee-147">Procházení pomocí Průzkumníku dat</span><span class="sxs-lookup"><span data-stu-id="24aee-147">Browse using the Data Explorer</span></span>

<span data-ttu-id="24aee-148">Teď se můžete vrátit do Průzkumníku dat na webu Azure Portal, procházet nová data grafu a zadávat na ně dotazy.</span><span class="sxs-lookup"><span data-stu-id="24aee-148">You can now go back to Data Explorer in the Azure portal and browse and query your new graph data.</span></span>

1. <span data-ttu-id="24aee-149">V Průzkumníku dat se nová databáze zobrazí v podokně Graphs (Grafy).</span><span class="sxs-lookup"><span data-stu-id="24aee-149">In Data Explorer, the new database appears in the Graphs pane.</span></span> <span data-ttu-id="24aee-150">Rozbalte položky **graphdb** a **graphcollz** a potom klikněte na **Graph**.</span><span class="sxs-lookup"><span data-stu-id="24aee-150">Expand **graphdb**, **graphcollz**, and then click **Graph**.</span></span>

2. <span data-ttu-id="24aee-151">Kliknutím na tlačítko **Použít filtr** použijte výchozí dotaz k zobrazení vrcholů v grafu.</span><span class="sxs-lookup"><span data-stu-id="24aee-151">Click the **Apply Filter** button to use the default query to view all the verticies in the graph.</span></span> <span data-ttu-id="24aee-152">V podokně Graphs (Grafy) se zobrazí data vygenerovaná ukázkovou aplikací.</span><span class="sxs-lookup"><span data-stu-id="24aee-152">The data generated by the sample app is displayed in the Graphs pane.</span></span>

    <span data-ttu-id="24aee-153">Graf můžete přiblížit nebo oddálit, můžete rozšířit prostor pro zobrazení grafu, přidat další vrcholy a přesouvat vrcholy na zobrazovacím povrchu.</span><span class="sxs-lookup"><span data-stu-id="24aee-153">You can zoom in and out of the graph, you can expand the graph display space, add additional verticies, and move verticies on the display surface.</span></span>

    ![Zobrazení grafu v Průzkumníku dat na webu Azure Portal](./media/create-graph-dotnet/graph-explorer.png)

## <a name="review-slas-in-the-azure-portal"></a><span data-ttu-id="24aee-155">Ověření smluv SLA na webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="24aee-155">Review SLAs in the Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="24aee-156">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="24aee-156">Clean up resources</span></span>

<span data-ttu-id="24aee-157">Pokud nebudete tuto aplikace nadále používat, odstraňte na základě následujícího postupu z portálu Azure Portal všechny prostředky vytvořené podle tohoto rychlého startu:</span><span class="sxs-lookup"><span data-stu-id="24aee-157">If you're not going to continue to use this app, delete all resources created by this quickstart in the Azure portal with the following steps:</span></span> 

1. <span data-ttu-id="24aee-158">V nabídce vlevo na portálu Azure Portal klikněte na **Skupiny prostředků** a pak klikněte na název vytvořeného prostředku.</span><span class="sxs-lookup"><span data-stu-id="24aee-158">From the left-hand menu in the Azure portal, click **Resource groups** and then click the name of the resource you created.</span></span> 
2. <span data-ttu-id="24aee-159">Na stránce skupiny prostředků klikněte na **Odstranit**, do textového pole zadejte prostředek, který chcete odstranit, a pak klikněte na **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="24aee-159">On your resource group page, click **Delete**, type the name of the resource to delete in the text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="24aee-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="24aee-160">Next steps</span></span>

<span data-ttu-id="24aee-161">V tomto rychlém startu jste se seznámili s postupem vytvoření účtu databáze Azure Cosmos, vytvoření grafu pomocí Průzkumníku dat a spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="24aee-161">In this quickstart, you've learned how to create an Azure Cosmos DB account, create a graph using the Data Explorer, and run an app.</span></span> <span data-ttu-id="24aee-162">Teď můžete pomocí konzoly Gremlin vytvářet složitější dotazy a implementovat účinnou logiku procházení grafů.</span><span class="sxs-lookup"><span data-stu-id="24aee-162">You can now build more complex queries and implement powerful graph traversal logic using Gremlin.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="24aee-163">Dotazování pomocí konzoly Gremlin</span><span class="sxs-lookup"><span data-stu-id="24aee-163">Query using Gremlin</span></span>](tutorial-query-graph.md)

