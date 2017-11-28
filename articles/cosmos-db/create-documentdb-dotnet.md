---
title: "Azure Cosmos DB: Sestavení webové aplikace pomocí rozhraní .NET a hello rozhraní API DocumentDB | Microsoft Docs"
description: "Uvede ukázku kódu .NET pomocí dotazu tooand tooconnect hello rozhraní API služby Azure Cosmos databáze DocumentDB"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: 35517e35d80c48662a51a99814652ffa1121fc5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-documentdb-api-web-app-with-net-and-hello-azure-portal"></a><span data-ttu-id="227b4-103">Azure Cosmos DB: Sestavení webové aplikace DocumentDB rozhraní API pomocí rozhraní .NET a hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="227b4-103">Azure Cosmos DB: Build a DocumentDB API web app with .NET and hello Azure portal</span></span>

<span data-ttu-id="227b4-104">Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku.</span><span class="sxs-lookup"><span data-stu-id="227b4-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="227b4-105">Můžete rychle vytvořit a dotazovat dokumentu, klíč/hodnota a graf databází, které těžit z globální distribuční hello a možnosti vodorovné škálování jádrem hello Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="227b4-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="227b4-106">Tento rychlý start předvádí, jak hello toocreate účet Azure Cosmos DB, dokumentu databáze a kolekce pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="227b4-106">This quick start demonstrates how toocreate an Azure Cosmos DB account, document database, and collection using hello Azure portal.</span></span> <span data-ttu-id="227b4-107">Budete pak sestavení a nasazení webové aplikace seznamu úkolů založený na hello [DocumentDB .NET API](documentdb-sdk-dotnet.md), jak ukazuje následující snímek obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="227b4-107">You'll then build and deploy a todo list web app built on hello [DocumentDB .NET API](documentdb-sdk-dotnet.md), as shown in hello following screenshot.</span></span> 

![Aplikace seznamu úkolů s ukázkovými daty](./media/create-documentdb-dotnet/azure-comosdb-todo-app-list.png)

## <a name="prerequisites"></a><span data-ttu-id="227b4-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="227b4-109">Prerequisites</span></span>

<span data-ttu-id="227b4-110">Pokud ještě nemáte nainstalované Visual Studio 2017, můžete stáhnout a použít hello **volné** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="227b4-110">If you don’t already have Visual Studio 2017 installed, you can download and use hello **free** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="227b4-111">Ujistěte se, že povolíte **Azure development** při instalaci sady Visual Studio hello.</span><span class="sxs-lookup"><span data-stu-id="227b4-111">Make sure that you enable **Azure development** during hello Visual Studio setup.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<a id="create-account"></a>
## <a name="create-a-database-account"></a><span data-ttu-id="227b4-112">Vytvoření účtu databáze</span><span class="sxs-lookup"><span data-stu-id="227b4-112">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

<a id="create-collection"></a>
## <a name="add-a-collection"></a><span data-ttu-id="227b4-113">Přidání kolekce</span><span class="sxs-lookup"><span data-stu-id="227b4-113">Add a collection</span></span>

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="add-sample-data"></a>
## <a name="add-sample-data"></a><span data-ttu-id="227b4-114">Přidání ukázkových dat</span><span class="sxs-lookup"><span data-stu-id="227b4-114">Add sample data</span></span>

<span data-ttu-id="227b4-115">Nyní můžete přidat tooyour nové shromažďování dat pomocí Průzkumníku dat.</span><span class="sxs-lookup"><span data-stu-id="227b4-115">You can now add data tooyour new collection using Data Explorer.</span></span>

1. <span data-ttu-id="227b4-116">V Průzkumníku dat hello nové databáze se zobrazí v podokně Kolekce hello.</span><span class="sxs-lookup"><span data-stu-id="227b4-116">In Data Explorer, hello new database appears in hello Collections pane.</span></span> <span data-ttu-id="227b4-117">Rozbalte hello **úlohy** databáze, rozbalte položku hello **položky** kolekce, klikněte na tlačítko **dokumenty**a potom klikněte na **nové dokumenty**.</span><span class="sxs-lookup"><span data-stu-id="227b4-117">Expand hello **Tasks** database, expand hello **Items** collection, click **Documents**, and then click **New Documents**.</span></span> 

   ![Vytváření nových dokumentů v Průzkumníku dat v hello portálu Azure](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-new-document.png)
  
2. <span data-ttu-id="227b4-119">Nyní přidejte toohello kolekce dokumentů s hello strukturu.</span><span class="sxs-lookup"><span data-stu-id="227b4-119">Now add a document toohello collection with hello following structure.</span></span>

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries.",
         "isComplete": false
     }
     ```

3. <span data-ttu-id="227b4-120">Po přidání hello json toohello **dokumenty** , klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="227b4-120">Once you've added hello json toohello **Documents** tab, click **Save**.</span></span>

    ![Kopírovat json data a klikněte na Uložit v Průzkumníku dat v hello portálu Azure](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-save-document.png)

4.  <span data-ttu-id="227b4-122">Vytvořte a uložte jeden další dokument, kde je vložit jedinečnou hodnotu pro hello `id` vlastnost a změňte hello ostatní vlastnosti podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="227b4-122">Create and save one more document where you insert a unique value for hello `id` property, and change hello other properties as you see fit.</span></span> <span data-ttu-id="227b4-123">Nové dokumenty můžou mít jakoukoli strukturu, protože Azure Cosmos DB neuplatňuje pro data žádné schéma.</span><span class="sxs-lookup"><span data-stu-id="227b4-123">Your new documents can have any structure you want as Azure Cosmos DB doesn't impose any schema on your data.</span></span>

     <span data-ttu-id="227b4-124">Nyní můžete vytvořit dotazy v Průzkumníku dat tooretrieve vaše data.</span><span class="sxs-lookup"><span data-stu-id="227b4-124">You can now use queries in Data Explorer tooretrieve your data.</span></span> <span data-ttu-id="227b4-125">Ve výchozím Průzkumníku dat používá `SELECT * FROM c` tooretrieve všechny dokumenty v hello kolekce, ale můžete změnit této tooa různých [dotazu SQL](documentdb-sql-query.md), jako například `SELECT * FROM c ORDER BY c._ts DESC`, tooreturn všechny dokumenty hello v sestupném pořadí podle jejich časové razítko.</span><span class="sxs-lookup"><span data-stu-id="227b4-125">By default, Data Explorer uses `SELECT * FROM c` tooretrieve all documents in hello collection, but you can change that tooa different [SQL query](documentdb-sql-query.md), such as `SELECT * FROM c ORDER BY c._ts DESC`, tooreturn all hello documents in descending order based on their timestamp.</span></span>
 
     <span data-ttu-id="227b4-126">Můžete také použít Průzkumníku dat toocreate uložené procedury, funkce UDF a aktivační události tooperform serverovou obchodní logiku také jako propustnost škálování.</span><span class="sxs-lookup"><span data-stu-id="227b4-126">You can also use Data Explorer toocreate stored procedures, UDFs, and triggers tooperform server-side business logic as well as scale throughput.</span></span> <span data-ttu-id="227b4-127">Průzkumník dat zpřístupní všechny hello předdefinované programový přístup k datům v hello rozhraní API k dispozici, ale poskytuje snadný přístup k datům tooyour v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="227b4-127">Data Explorer exposes all of hello built-in programmatic data access available in hello APIs, but provides easy access tooyour data in hello Azure portal.</span></span>

## <a name="clone-hello-sample-application"></a><span data-ttu-id="227b4-128">Klonování hello ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="227b4-128">Clone hello sample application</span></span>

<span data-ttu-id="227b4-129">Teď umožňuje přepnout tooworking s kódem.</span><span class="sxs-lookup"><span data-stu-id="227b4-129">Now let's switch tooworking with code.</span></span> <span data-ttu-id="227b4-130">Pojďme klonovat aplikace DocumentDB API z Githubu, nastavte hello připojovací řetězec a spusťte ho.</span><span class="sxs-lookup"><span data-stu-id="227b4-130">Let's clone a DocumentDB API app from GitHub, set hello connection string, and run it.</span></span> <span data-ttu-id="227b4-131">Uvidíte, jak je snadné toowork s daty prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="227b4-131">You'll see how easy it is toowork with data programmatically.</span></span> 

1. <span data-ttu-id="227b4-132">Otevřete okno terminálu git, jako je například git bash a `CD` tooa pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="227b4-132">Open a git terminal window, such as git bash, and `CD` tooa working directory.</span></span>  

2. <span data-ttu-id="227b4-133">Spusťte následující příkaz tooclone hello Ukázka úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="227b4-133">Run hello following command tooclone hello sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/documentdb-dotnet-todo-app.git
    ```

3. <span data-ttu-id="227b4-134">Poté otevřete soubor řešení hello todo v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="227b4-134">Then open hello todo solution file in Visual Studio.</span></span> 

## <a name="review-hello-code"></a><span data-ttu-id="227b4-135">Zkontrolujte hello kódu</span><span class="sxs-lookup"><span data-stu-id="227b4-135">Review hello code</span></span>

<span data-ttu-id="227b4-136">Provedeme jejich stručný přehled o dění v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="227b4-136">Let's make a quick review of what's happening in hello app.</span></span> <span data-ttu-id="227b4-137">Otevřete hello DocumentDBRepository.cs souboru a najdete v ní vytvořit tyto řádky kódu hello prostředky Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="227b4-137">Open hello DocumentDBRepository.cs file and you'll find that these lines of code create hello Azure Cosmos DB resources.</span></span> 

* <span data-ttu-id="227b4-138">na řádku 73 je inicializován Hello DocumentClient.</span><span class="sxs-lookup"><span data-stu-id="227b4-138">hello DocumentClient is initialized on line 73.</span></span>

    ```csharp
    client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpoint"]), ConfigurationManager.AppSettings["authKey"]);`
    ```

* <span data-ttu-id="227b4-139">Na řádku 88 se vytvoří nová databáze.</span><span class="sxs-lookup"><span data-stu-id="227b4-139">A new database is created on line 88.</span></span>

    ```csharp
    await client.CreateDatabaseAsync(new Database { Id = DatabaseId });
    ```

* <span data-ttu-id="227b4-140">Na řádku 107 se vytvoří nová kolekce.</span><span class="sxs-lookup"><span data-stu-id="227b4-140">A new collection is created on line 107.</span></span>

    ```csharp
    await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri(DatabaseId),
        new DocumentCollection { Id = CollectionId },
        new RequestOptions { OfferThroughput = 1000 });
    ```

## <a name="update-your-connection-string"></a><span data-ttu-id="227b4-141">Aktualizace připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="227b4-141">Update your connection string</span></span>

<span data-ttu-id="227b4-142">Nyní přejděte zpět toohello Azure portálu tooget vaše informace o připojovacím řetězci a zkopírujte jej do aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="227b4-142">Now go back toohello Azure portal tooget your connection string information and copy it into hello app.</span></span>

1. <span data-ttu-id="227b4-143">V hello [portál Azure](http://portal.azure.com/), ve vašem Azure Cosmos DB účet, klikněte v levé navigační hello **klíče**a potom klikněte na **klíče pro čtení a zápis**.</span><span class="sxs-lookup"><span data-stu-id="227b4-143">In hello [Azure portal](http://portal.azure.com/), in your Azure Cosmos DB account, in hello left navigation click **Keys**, and then click **Read-write Keys**.</span></span> <span data-ttu-id="227b4-144">Do souboru web.config hello v dalším kroku hello použijete hello tlačítka Kopírovat na pravé straně hello hello obrazovky toocopy hello URI a primární klíč.</span><span class="sxs-lookup"><span data-stu-id="227b4-144">You'll use hello copy buttons on hello right side of hello screen toocopy hello URI and Primary Key into hello web.config file in hello next step.</span></span>

    ![Zobrazení a zkopírování přístupový klíč v hello portálu Azure, okna klíče](./media/create-documentdb-dotnet/keys.png)

2. <span data-ttu-id="227b4-146">V aplikaci Visual Studio 2017 otevřete soubor web.config hello.</span><span class="sxs-lookup"><span data-stu-id="227b4-146">In Visual Studio 2017, open hello web.config file.</span></span> 

3. <span data-ttu-id="227b4-147">Zkopírujte URI hodnota z portálu hello (pomocí tlačítka kopírování hello) a nastavit jej jako hello hodnotu klíče hello koncový bod v souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="227b4-147">Copy your URI value from hello portal (using hello copy button) and make it hello value of hello endpoint key in web.config.</span></span> 

    `<add key="endpoint" value="FILLME" />`

4. <span data-ttu-id="227b4-148">Poté zkopírujte primární klíč hodnota z portálu hello a nastavit jej jako hello hodnotu hello authKey v souboru web.config. Jste nyní aktualizovat vaši aplikaci s všechny údaje hello potřebuje toocommunicate s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="227b4-148">Then copy your PRIMARY KEY value from hello portal and make it hello value of hello authKey in web.config. You've now updated your app with all hello info it needs toocommunicate with Azure Cosmos DB.</span></span> 

    `<add key="authKey" value="FILLME" />`
    
## <a name="run-hello-web-app"></a><span data-ttu-id="227b4-149">Spouštění hello webové aplikace</span><span class="sxs-lookup"><span data-stu-id="227b4-149">Run hello web app</span></span>
1. <span data-ttu-id="227b4-150">V sadě Visual Studio, klikněte pravým tlačítkem na projekt hello v **Průzkumníku řešení** a pak klikněte na **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="227b4-150">In Visual Studio, right-click on hello project in **Solution Explorer** and then click **Manage NuGet Packages**.</span></span> 

2. <span data-ttu-id="227b4-151">V hello NuGet **Procházet** zadejte *DocumentDB*.</span><span class="sxs-lookup"><span data-stu-id="227b4-151">In hello NuGet **Browse** box, type *DocumentDB*.</span></span>

3. <span data-ttu-id="227b4-152">Z výsledků hello nainstalovat hello **Microsoft.Azure.DocumentDB** knihovny.</span><span class="sxs-lookup"><span data-stu-id="227b4-152">From hello results, install hello **Microsoft.Azure.DocumentDB** library.</span></span> <span data-ttu-id="227b4-153">Tím se nainstaluje balíček Microsoft.Azure.DocumentDB hello a také všechny závislosti.</span><span class="sxs-lookup"><span data-stu-id="227b4-153">This installs hello Microsoft.Azure.DocumentDB package as well as all dependencies.</span></span>

4. <span data-ttu-id="227b4-154">Klikněte na kombinaci kláves CTRL + F5 toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="227b4-154">Click CTRL + F5 toorun hello application.</span></span> <span data-ttu-id="227b4-155">Aplikace se zobrazí v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="227b4-155">Your app displays in your browser.</span></span> 

5. <span data-ttu-id="227b4-156">Klikněte na tlačítko **vytvořit nový** v hello prohlížeče a vytvořit pár nové úlohy ve vaší aplikaci seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="227b4-156">Click **Create New** in hello browser and create a few new tasks in your to-do app.</span></span>

   ![Aplikace seznamu úkolů s ukázkovými daty](./media/create-documentdb-dotnet/azure-comosdb-todo-app-list.png)

<span data-ttu-id="227b4-158">Teď můžete přejít zpět tooData Průzkumníka a zobrazit dotaz, upravit a pracovat s Tato nová data.</span><span class="sxs-lookup"><span data-stu-id="227b4-158">You can now go back tooData Explorer and see query, modify, and work with this new data.</span></span> 

## <a name="review-slas-in-hello-azure-portal"></a><span data-ttu-id="227b4-159">Zkontrolujte SLA v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="227b4-159">Review SLAs in hello Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="227b4-160">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="227b4-160">Clean up resources</span></span>

<span data-ttu-id="227b4-161">Pokud ale nebudete toocontinue toouse této aplikace, odstraňte všechny prostředky, které jsou vytvořené tento rychlý start v hello portál Azure s hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="227b4-161">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span>

1. <span data-ttu-id="227b4-162">V levé nabídce hello v hello portálu Azure klikněte na **skupiny prostředků** a pak klikněte na název hello hello prostředků, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="227b4-162">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="227b4-163">Na stránce skupiny prostředků, klikněte na tlačítko **odstranit**hello textového pole zadejte název hello toodelete hello prostředků a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="227b4-163">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="227b4-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="227b4-164">Next steps</span></span>

<span data-ttu-id="227b4-165">V tento rychlý start když jste se naučili jak toocreate účtu Azure Cosmos DB, vytvořte kolekci pomocí hello Průzkumníku dat a spusťte webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="227b4-165">In this quickstart, you've learned how toocreate an Azure Cosmos DB account, create a collection using hello Data Explorer, and run a web app.</span></span> <span data-ttu-id="227b4-166">Nyní můžete importovat další data tooyour Cosmos DB účtu.</span><span class="sxs-lookup"><span data-stu-id="227b4-166">You can now import additional data tooyour Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="227b4-167">Importování dat do služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="227b4-167">Import data into Azure Cosmos DB</span></span>](import-data.md)


