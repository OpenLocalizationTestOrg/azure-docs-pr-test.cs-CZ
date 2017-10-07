---
title: "hello aaaBuild aplikace Azure Cosmos DB .NET pomocí rozhraní API tabulky | Microsoft Docs"
description: "Začínáme s rozhraním API tabulek Azure Cosmos DB pomocí technologie .NET"
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
ms.assetid: 66327041-4d5e-4ce6-a394-fee107c18e59
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 06/22/2017
ms.author: arramac
ms.openlocfilehash: bdd4f8ec45407962b3d2cb26aa814a20cfc62173
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-net-application-using-hello-table-api"></a><span data-ttu-id="4c02a-103">Azure Cosmos DB: Vytvoření aplikace .NET pomocí hello tabulky rozhraní API</span><span class="sxs-lookup"><span data-stu-id="4c02a-103">Azure Cosmos DB: Build a .NET application using hello Table API</span></span>

<span data-ttu-id="4c02a-104">Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku.</span><span class="sxs-lookup"><span data-stu-id="4c02a-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="4c02a-105">Můžete rychle vytvořit a dotazovat dokumentu, klíč/hodnota a graf databází, které těžit z globální distribuční hello a možnosti vodorovné škálování jádrem hello Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="4c02a-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="4c02a-106">Tento rychlý start předvádí, jak toocreate Azure DB Cosmos účtu a vytvořte tabulku v rámci tohoto účtu pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4c02a-106">This quick start demonstrates how toocreate an Azure Cosmos DB account, and create a table within that account using hello Azure portal.</span></span> <span data-ttu-id="4c02a-107">Budete pak napíšete kód tooinsert, aktualizace a odstranění entity a spustit některé dotazy pomocí hello nové [Windows Azure Storage úrovně Premium Table](https://aka.ms/premiumtablenuget) (preview) balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="4c02a-107">You'll then write code tooinsert, update, and delete entities, and run some queries using hello new [Windows Azure Storage Premium Table](https://aka.ms/premiumtablenuget) (preview) package from NuGet.</span></span> <span data-ttu-id="4c02a-108">V této knihovně bylo hello stejné třídy a metody podpisy jako veřejné hello [sada SDK úložiště Windows Azure](https://www.nuget.org/packages/WindowsAzure.Storage), ale také obsahuje hello možnost tooconnect tooAzure Cosmos DB účty pomocí hello [tabulky API](table-introduction.md) (preview).</span><span class="sxs-lookup"><span data-stu-id="4c02a-108">This library has hello same classes and method signatures as hello public [Windows Azure Storage SDK](https://www.nuget.org/packages/WindowsAzure.Storage), but also has hello ability tooconnect tooAzure Cosmos DB accounts using hello [Table API](table-introduction.md) (preview).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="4c02a-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4c02a-109">Prerequisites</span></span>

<span data-ttu-id="4c02a-110">Pokud ještě nemáte nainstalované Visual Studio 2017, můžete stáhnout a použít hello **volné** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="4c02a-110">If you don’t already have Visual Studio 2017 installed, you can download and use hello **free** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="4c02a-111">Ujistěte se, že povolíte **Azure development** při instalaci sady Visual Studio hello.</span><span class="sxs-lookup"><span data-stu-id="4c02a-111">Make sure that you enable **Azure development** during hello Visual Studio setup.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="4c02a-112">Vytvoření účtu databáze</span><span class="sxs-lookup"><span data-stu-id="4c02a-112">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)]

## <a name="add-a-table"></a><span data-ttu-id="4c02a-113">Přidání tabulky</span><span class="sxs-lookup"><span data-stu-id="4c02a-113">Add a table</span></span>

[!INCLUDE [cosmos-db-create-table](../../includes/cosmos-db-create-table.md)]

## <a name="add-sample-data"></a><span data-ttu-id="4c02a-114">Přidání ukázkových dat</span><span class="sxs-lookup"><span data-stu-id="4c02a-114">Add sample data</span></span>

<span data-ttu-id="4c02a-115">Nyní můžete přidat data tooyour nové tabulky pomocí Průzkumníku dat (Preview).</span><span class="sxs-lookup"><span data-stu-id="4c02a-115">You can now add data tooyour new table using Data Explorer (Preview).</span></span>

1. <span data-ttu-id="4c02a-116">V Průzkumníku dat rozbalte **ukázkovou tabulku**, klikněte na **Entity** a potom klikněte na **Přidat entitu**.</span><span class="sxs-lookup"><span data-stu-id="4c02a-116">In Data Explorer, expand **sample-table**, click **Entities**, and then click **Add Entity**.</span></span>

   ![Vytvořte nové entity v Průzkumníku dat v hello portálu Azure](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-document.png)
2. <span data-ttu-id="4c02a-118">Nyní přidejte dat toohello PartitionKey hodnota pole a hodnota pole RowKey a klikněte na **Přidat entitu**.</span><span class="sxs-lookup"><span data-stu-id="4c02a-118">Now add data toohello PartitionKey value box and RowKey value box, and click **Add Entity**.</span></span>

   ![Nastavit hello klíč oddílu a klíč řádku pro nové entity](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-entity.png)
  
    <span data-ttu-id="4c02a-120">Můžete nyní přidat další tabulky tooyour entity, upravit vaší entity nebo dotazování na data v Průzkumníku dat.</span><span class="sxs-lookup"><span data-stu-id="4c02a-120">You can now add more entities tooyour table, edit your entities, or query your data in Data Explorer.</span></span> <span data-ttu-id="4c02a-121">Průzkumník dat je také, kde můžete škálovat vaše propustnost a přidání uložené procedury, uživatelem definované funkce a tabulka tooyour aktivační události.</span><span class="sxs-lookup"><span data-stu-id="4c02a-121">Data Explorer is also where you can scale your throughput and add stored procedures, user defined functions, and triggers tooyour table.</span></span>

## <a name="clone-hello-sample-application"></a><span data-ttu-id="4c02a-122">Klonování hello ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="4c02a-122">Clone hello sample application</span></span>

<span data-ttu-id="4c02a-123">Nyní Pojďme klonovat aplikace tabulky z githubu, nastavte hello připojovací řetězec a spusťte ho.</span><span class="sxs-lookup"><span data-stu-id="4c02a-123">Now let's clone a Table app from github, set hello connection string, and run it.</span></span> <span data-ttu-id="4c02a-124">Uvidíte, jak je snadné toowork s daty prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="4c02a-124">You'll see how easy it is toowork with data programmatically.</span></span> 

1. <span data-ttu-id="4c02a-125">Otevřete okno terminálu git, jako je například git bash a `cd` tooa pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="4c02a-125">Open a git terminal window, such as git bash, and `cd` tooa working directory.</span></span>  

2. <span data-ttu-id="4c02a-126">Spusťte následující příkaz tooclone hello Ukázka úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="4c02a-126">Run hello following command tooclone hello sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-table-dotnet-getting-started.git
    ```

3. <span data-ttu-id="4c02a-127">Poté otevřete soubor řešení hello v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4c02a-127">Then open hello solution file in Visual Studio.</span></span> 

## <a name="review-hello-code"></a><span data-ttu-id="4c02a-128">Zkontrolujte hello kódu</span><span class="sxs-lookup"><span data-stu-id="4c02a-128">Review hello code</span></span>

<span data-ttu-id="4c02a-129">Provedeme jejich stručný přehled o dění v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="4c02a-129">Let's make a quick review of what's happening in hello app.</span></span> <span data-ttu-id="4c02a-130">Hello otevřete soubor Program.cs a najdete, že tyto řádky kódu vytvořit hello prostředky Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="4c02a-130">Open hello Program.cs file and you'll find that these lines of code create hello Azure Cosmos DB resources.</span></span> 

* <span data-ttu-id="4c02a-131">Hello CloudTableClient je inicializován.</span><span class="sxs-lookup"><span data-stu-id="4c02a-131">hello CloudTableClient is initialized.</span></span>

    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString); 
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

* <span data-ttu-id="4c02a-132">Vytvoří se nová tabulka, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="4c02a-132">A new table is created if it does not exist.</span></span>

    ```csharp
    CloudTable table = tableClient.GetTableReference("people");
    table.CreateIfNotExists();
    ```

* <span data-ttu-id="4c02a-133">Vytvoří se nový kontejner tabulky.</span><span class="sxs-lookup"><span data-stu-id="4c02a-133">A new Table container is created.</span></span> <span data-ttu-id="4c02a-134">Si všimnete, že je velmi podobné tooregular tento kód Azure Table storage SDK.</span><span class="sxs-lookup"><span data-stu-id="4c02a-134">You will notice this code very similar tooregular Azure Table storage SDK.</span></span> 

    ```csharp
    CustomerEntity item = new CustomerEntity()
                {
                    PartitionKey = Guid.NewGuid().ToString(),
                    RowKey = Guid.NewGuid().ToString(),
                    Email = $"{GetRandomString(6)}@contoso.com",
                    PhoneNumber = "425-555-0102",
                    Bio = GetRandomString(1000)
                };
    ```

## <a name="update-your-connection-string"></a><span data-ttu-id="4c02a-135">Aktualizace připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="4c02a-135">Update your connection string</span></span>

<span data-ttu-id="4c02a-136">Nyní jsme budete aktualizovat informace o připojovacím řetězci hello tak vaše aplikace může kontaktovat tooAzure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="4c02a-136">Now we'll update hello connection string information so your app can talk tooAzure Cosmos DB.</span></span> 

1. <span data-ttu-id="4c02a-137">V sadě Visual Studio otevřete soubor app.config hello.</span><span class="sxs-lookup"><span data-stu-id="4c02a-137">In Visual Studio, open hello app.config file.</span></span> 

2. <span data-ttu-id="4c02a-138">V hello [portál Azure](http://portal.azure.com/)v hello Azure Cosmos DB levé navigační nabídky, klikněte na tlačítko **připojovací řetězec**.</span><span class="sxs-lookup"><span data-stu-id="4c02a-138">In hello [Azure portal](http://portal.azure.com/), in hello Azure Cosmos DB left navigation menu, click **Connection String**.</span></span> <span data-ttu-id="4c02a-139">Potom v podokně nové hello klikněte na tlačítko Kopírovat hello pro hello připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="4c02a-139">Then in hello new pane click hello copy button for hello connection string.</span></span> 

    ![Zobrazení a zkopírování hello koncový bod a klíč účtu v podokně hello připojovací řetězec](./media/create-table-dotnet/keys.png)

3. <span data-ttu-id="4c02a-141">Jako hodnotu hello hello PremiumStorageConnectionString vložit hodnotu hello do souboru app.config hello.</span><span class="sxs-lookup"><span data-stu-id="4c02a-141">Paste hello value into hello app.config file as hello value of hello PremiumStorageConnectionString.</span></span> 

    `<add key="PremiumStorageConnectionString" 
        value="DefaultEndpointsProtocol=https;AccountName=MYSTORAGEACCOUNT;AccountKey=AUTHKEY;TableEndpoint=https://COSMOSDB.documents.azure.com" />`    

    <span data-ttu-id="4c02a-142">Můžete ponechat hello StandardStorageConnectionString, jako je.</span><span class="sxs-lookup"><span data-stu-id="4c02a-142">You can leave hello StandardStorageConnectionString as is.</span></span>

<span data-ttu-id="4c02a-143">Jste nyní aktualizovat vaši aplikaci s všechny údaje hello potřebuje toocommunicate s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="4c02a-143">You've now updated your app with all hello info it needs toocommunicate with Azure Cosmos DB.</span></span> 

## <a name="run-hello-web-app"></a><span data-ttu-id="4c02a-144">Spouštění hello webové aplikace</span><span class="sxs-lookup"><span data-stu-id="4c02a-144">Run hello web app</span></span>

1. <span data-ttu-id="4c02a-145">V sadě Visual Studio, klikněte pravým tlačítkem na hello **PremiumTableGetStarted** projektu v **Průzkumníku řešení** a pak klikněte na **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="4c02a-145">In Visual Studio, right-click on hello **PremiumTableGetStarted** project in **Solution Explorer** and then click **Manage NuGet Packages**.</span></span> 

2. <span data-ttu-id="4c02a-146">V hello NuGet **Procházet** zadejte *WindowsAzure.Storage PremiumTable*.</span><span class="sxs-lookup"><span data-stu-id="4c02a-146">In hello NuGet **Browse** box, type *WindowsAzure.Storage-PremiumTable*.</span></span>

3. <span data-ttu-id="4c02a-147">Zkontrolujte hello **zahrnout předběžné verze** pole.</span><span class="sxs-lookup"><span data-stu-id="4c02a-147">Check hello **Include prerelease** box.</span></span> 

4. <span data-ttu-id="4c02a-148">Z výsledků hello nainstalovat hello **WindowsAzure.Storage PremiumTable** knihovny.</span><span class="sxs-lookup"><span data-stu-id="4c02a-148">From hello results, install hello **WindowsAzure.Storage-PremiumTable** library.</span></span> <span data-ttu-id="4c02a-149">Tím se nainstaluje hello preview rozhraní API služby Azure Cosmos DB tabulky balíčku a také všechny závislosti.</span><span class="sxs-lookup"><span data-stu-id="4c02a-149">This installs hello preview Azure Cosmos DB Table API package as well as all dependencies.</span></span> <span data-ttu-id="4c02a-150">Všimněte si, že se jedná o jiný balíček NuGet než balíček Windows Azure Storage hello používá Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="4c02a-150">Note that this is a different NuGet package than hello Windows Azure Storage package used by Azure Table storage.</span></span> 

5. <span data-ttu-id="4c02a-151">Klikněte na kombinaci kláves CTRL + F5 toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="4c02a-151">Click CTRL + F5 toorun hello application.</span></span>

    <span data-ttu-id="4c02a-152">okno konzoly Hello zobrazí data hello právě přidali, načíst, dotaz, nahradit a odstraněna z tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="4c02a-152">hello console window displays hello data being added, retrieved, queried, replaced and deleted from hello table.</span></span> <span data-ttu-id="4c02a-153">Po dokončení skriptu hello stiskněte okna konzoly všechny klíče tooclose hello.</span><span class="sxs-lookup"><span data-stu-id="4c02a-153">When hello script completes, press any key tooclose hello console window.</span></span> 
    
    ![Výstup konzoly z hello rychlý start](./media/create-table-dotnet/azure-cosmosdb-table-quickstart-console-output.png)

6. <span data-ttu-id="4c02a-155">Pokud chcete, aby toosee hello nové entity v Průzkumníku dat, jenom komentář 188 208 řádků v souboru program.cs, nejsou odstraněny, pak znovu spusťte ukázkové hello.</span><span class="sxs-lookup"><span data-stu-id="4c02a-155">If you want toosee hello new entities in Data Explorer, just comment out lines 188-208 in program.cs so they aren't deleted, then run hello sample again.</span></span> 

    <span data-ttu-id="4c02a-156">Můžete teď se vrátit tooData Explorer, klikněte na tlačítko **aktualizovat**, rozbalte hello **osoby** tabulky a klikněte na tlačítko **entity**a pracovat s Tato nová data.</span><span class="sxs-lookup"><span data-stu-id="4c02a-156">You can now go back tooData Explorer, click **Refresh**, expand hello **people** table and click **Entities**, and then work with this new data.</span></span> 

    ![Nové entity v Průzkumníku dat](./media/create-table-dotnet/azure-cosmosdb-table-quickstart-data-explorer.png)

## <a name="review-slas-in-hello-azure-portal"></a><span data-ttu-id="4c02a-158">Zkontrolujte SLA v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="4c02a-158">Review SLAs in hello Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="4c02a-159">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="4c02a-159">Clean up resources</span></span>

<span data-ttu-id="4c02a-160">Pokud ale nebudete toocontinue toouse této aplikace, odstraňte všechny prostředky, které jsou vytvořené tento rychlý start v hello portál Azure s hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="4c02a-160">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span> 

1. <span data-ttu-id="4c02a-161">V levé nabídce hello v hello portálu Azure klikněte na **skupiny prostředků** a pak klikněte na název hello hello prostředků, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="4c02a-161">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="4c02a-162">Na stránce skupiny prostředků, klikněte na tlačítko **odstranit**hello textového pole zadejte název hello toodelete hello prostředků a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="4c02a-162">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4c02a-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4c02a-163">Next steps</span></span>

<span data-ttu-id="4c02a-164">V tento rychlý start když jste se naučili toocreate účet Azure Cosmos DB vytvoření tabulky s použitím hello Průzkumníku dat a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4c02a-164">In this quickstart, you've learned how toocreate an Azure Cosmos DB account, create a table using hello Data Explorer, and run an app.</span></span>  <span data-ttu-id="4c02a-165">Teď se můžete dotazovat svá data pomocí funkce hello tabulky rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4c02a-165">Now you can query your data using hello Table API.</span></span>  

> [!div class="nextstepaction"]
> [<span data-ttu-id="4c02a-166">Dotaz pomocí hello tabulky rozhraní API</span><span class="sxs-lookup"><span data-stu-id="4c02a-166">Query using hello Table API</span></span>](tutorial-query-table.md)

