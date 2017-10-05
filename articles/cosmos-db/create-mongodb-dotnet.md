---
title: "Databáze Azure Cosmos: Sestavení webové aplikace v prostředí .NET s rozhraním API MongoDB | Dokumentace Microsoftu"
description: "Představuje ukázku kódu .NET, kterou můžete použít k připojení a dotazování do rozhraní API MongoDB databáze Azure Cosmos."
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
ms.openlocfilehash: 2d30bec75d701b1fd55355d1e139350b6d828c9a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cosmos-db-build-a-mongodb-api-web-app-with-net-and-the-azure-portal"></a><span data-ttu-id="c17c9-103">Databáze Azure Cosmos: Sestavení webové aplikace s rozhraním API MongoDB v prostředí .NET a na webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c17c9-103">Azure Cosmos DB: Build a MongoDB API web app with .NET and the Azure portal</span></span>

<span data-ttu-id="c17c9-104">Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku.</span><span class="sxs-lookup"><span data-stu-id="c17c9-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="c17c9-105">Můžete snadno vytvořit a dotazovat databáze dotazů, klíčů/hodnot a grafů, které tak můžou využívat výhody použitelnosti v celosvětovém měřítku a možností horizontálního škálování v jádru Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="c17c9-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from the global distribution and horizontal scale capabilities at the core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="c17c9-106">Tento rychlý start popisuje způsob vytvoření účtu databáze Azure Cosmos, databáze dokumentů a kolekce pomocí webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="c17c9-106">This quick start demonstrates how to create an Azure Cosmos DB account, document database, and collection using the Azure portal.</span></span> <span data-ttu-id="c17c9-107">Potom sestavíte a nasadíte webovou aplikaci seznamu úkolů založenou na [ovladači .NET MongoDB](https://docs.mongodb.com/ecosystem/drivers/csharp/).</span><span class="sxs-lookup"><span data-stu-id="c17c9-107">You'll then build and deploy a tasks list web app built on the [MongoDB .NET driver](https://docs.mongodb.com/ecosystem/drivers/csharp/).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="c17c9-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c17c9-108">Prerequisites</span></span>

<span data-ttu-id="c17c9-109">Pokud ještě nemáte nainstalovanou sadu Visual Studio 2017, můžete stáhnout a použít **bezplatnou verzi** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="c17c9-109">If you don’t already have Visual Studio 2017 installed, you can download and use the **free** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="c17c9-110">Nezapomeňte při instalaci sady Visual Studio povolit možnost **Azure Development**.</span><span class="sxs-lookup"><span data-stu-id="c17c9-110">Make sure that you enable **Azure development** during the Visual Studio setup.</span></span>
[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<a id="create-account"></a>
## <a name="create-a-database-account"></a><span data-ttu-id="c17c9-111">Vytvoření účtu databáze</span><span class="sxs-lookup"><span data-stu-id="c17c9-111">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="clone-the-sample-application"></a><span data-ttu-id="c17c9-112">Klonování ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="c17c9-112">Clone the sample application</span></span>

<span data-ttu-id="c17c9-113">Teď naklonujeme aplikaci rozhraní API MongoDB z GitHubu, nastavíme připojovací řetězec a spustíme ji.</span><span class="sxs-lookup"><span data-stu-id="c17c9-113">Now let's clone a MongoDB API app from github, set the connection string, and run it.</span></span> <span data-ttu-id="c17c9-114">Přesvědčíte se, jak snadno se pracuje s daty prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="c17c9-114">You'll see how easy it is to work with data programmatically.</span></span> 

1. <span data-ttu-id="c17c9-115">Otevřete okno terminálu Git, jako je třeba Git Bash, a pomocí `cd` přejděte do pracovního adresáře.</span><span class="sxs-lookup"><span data-stu-id="c17c9-115">Open a git terminal window, such as git bash, and `cd` to a working directory.</span></span>  

2. <span data-ttu-id="c17c9-116">Ukázkové úložiště naklonujete spuštěním následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="c17c9-116">Run the following command to clone the sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-getting-started.git
    ```

3. <span data-ttu-id="c17c9-117">Potom otevřete soubor řešení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c17c9-117">Then open the solution file in Visual Studio.</span></span> 

## <a name="review-the-code"></a><span data-ttu-id="c17c9-118">Kontrola kódu</span><span class="sxs-lookup"><span data-stu-id="c17c9-118">Review the code</span></span>

<span data-ttu-id="c17c9-119">Ještě jednou se stručně podívejme na to, co se v aplikaci děje.</span><span class="sxs-lookup"><span data-stu-id="c17c9-119">Let's make a quick review of what's happening in the app.</span></span> <span data-ttu-id="c17c9-120">Otevřete soubor **Dal.cs** v adresáři **DAL** a zjistíte, že tyto řádky kódu vytvářejí prostředky databáze Azure Cosmos.</span><span class="sxs-lookup"><span data-stu-id="c17c9-120">Open the **Dal.cs** file under the **DAL** directory and you'll find that these lines of code create the Azure Cosmos DB resources.</span></span> 

* <span data-ttu-id="c17c9-121">Inicializuje se klient Mongo.</span><span class="sxs-lookup"><span data-stu-id="c17c9-121">Initialize the Mongo Client.</span></span>

    ```cs
        MongoClientSettings settings = new MongoClientSettings();
        settings.Server = new MongoServerAddress(host, 10255);
        settings.UseSsl = true;
        settings.SslSettings = new SslSettings();
        settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;

        MongoIdentity identity = new MongoInternalIdentity(dbName, userName);
        MongoIdentityEvidence evidence = new PasswordEvidence(password);

        settings.Credentials = new List<MongoCredential>()
        {
            new MongoCredential("SCRAM-SHA-1", identity, evidence)
        };

        MongoClient client = new MongoClient(settings);
    ```

* <span data-ttu-id="c17c9-122">Načte se databáze a kolekce.</span><span class="sxs-lookup"><span data-stu-id="c17c9-122">Retrieve the database and the collection.</span></span>

    ```cs
    private string dbName = "Tasks";
    private string collectionName = "TasksList";

    var database = client.GetDatabase(dbName);
    var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
    ```

* <span data-ttu-id="c17c9-123">Načtou se všechny dokumenty.</span><span class="sxs-lookup"><span data-stu-id="c17c9-123">Retrieve all documents.</span></span>

    ```cs
    collection.Find(new BsonDocument()).ToList();
    ```

## <a name="update-your-connection-string"></a><span data-ttu-id="c17c9-124">Aktualizace připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="c17c9-124">Update your connection string</span></span>

<span data-ttu-id="c17c9-125">Teď se vraťte zpátky na web Azure Portal, kde najdete informace o připojovacím řetězci, a zkopírujte je do aplikace.</span><span class="sxs-lookup"><span data-stu-id="c17c9-125">Now go back to the Azure portal to get your connection string information and copy it into the app.</span></span>

1. <span data-ttu-id="c17c9-126">Na webu [Azure Portal](http://portal.azure.com/) klikněte v účtu databáze Azure Cosmos v levém navigačním panelu na možnost **Připojovací řetězec** a potom klikněte na **Klíče pro čtení i zápis**.</span><span class="sxs-lookup"><span data-stu-id="c17c9-126">In the [Azure portal](http://portal.azure.com/), in your Azure Cosmos DB account, in the left navigation click **Connection String**, and then click **Read-write Keys**.</span></span> <span data-ttu-id="c17c9-127">V dalším kroku zkopírujete pomocí tlačítek kopírování na pravé straně obrazovky uživatelské jméno, heslo a hostitele do souboru Dal.cs.</span><span class="sxs-lookup"><span data-stu-id="c17c9-127">You'll use the copy buttons on the right side of the screen to copy the Username, Password, and Host into the Dal.cs file in the next step.</span></span>

2. <span data-ttu-id="c17c9-128">Otevřete soubor **Dal.cs** v adresáři **DAL**.</span><span class="sxs-lookup"><span data-stu-id="c17c9-128">Open the **Dal.cs** file in the **DAL** directory.</span></span> 

3. <span data-ttu-id="c17c9-129">Z portálu zkopírujte hodnotu **username** (pomocí tlačítka kopírování) a nastavte ji jako hodnotu položky **username** v souboru **Dal.cs**.</span><span class="sxs-lookup"><span data-stu-id="c17c9-129">Copy your **username** value from the portal (using the copy button) and make it the value of the **username** in your **Dal.cs** file.</span></span> 

4. <span data-ttu-id="c17c9-130">Potom z portálu zkopírujte hodnotu **host** a nastavte ji jako hodnotu **host** v souboru **Dal.cs**.</span><span class="sxs-lookup"><span data-stu-id="c17c9-130">Then copy your **host** value from the portal and make it the value of the **host** in your **Dal.cs** file.</span></span> 

5. <span data-ttu-id="c17c9-131">Nakonec z portálu zkopírujte hodnotu **password** a nastavte ji jako hodnotu **password** v souboru **Dal.cs**.</span><span class="sxs-lookup"><span data-stu-id="c17c9-131">Finally copy your **password** value from the portal and make it the value of the **password** in your **Dal.cs** file.</span></span> 

<span data-ttu-id="c17c9-132">Teď jste aktualizovali aplikaci a zadali do ní všechny informace potřebné ke komunikaci s databází Azure Cosmos.</span><span class="sxs-lookup"><span data-stu-id="c17c9-132">You've now updated your app with all the info it needs to communicate with Azure Cosmos DB.</span></span> 
    
## <a name="run-the-web-app"></a><span data-ttu-id="c17c9-133">Spuštění webové aplikace</span><span class="sxs-lookup"><span data-stu-id="c17c9-133">Run the web app</span></span>

1. <span data-ttu-id="c17c9-134">V sadě Visual Studio klikněte v **Průzkumníku řešení** pravým tlačítkem myši na projekt a potom klikněte na **Spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c17c9-134">In Visual Studio, right-click on the project in **Solution Explorer** and then click **Manage NuGet Packages**.</span></span> 

2. <span data-ttu-id="c17c9-135">Do pole **Procházet** v NuGetu zadejte *MongoDB.Driver*.</span><span class="sxs-lookup"><span data-stu-id="c17c9-135">In the NuGet **Browse** box, type *MongoDB.Driver*.</span></span>

3. <span data-ttu-id="c17c9-136">Z výsledků nainstalujte knihovnu **MongoDB.Driver**.</span><span class="sxs-lookup"><span data-stu-id="c17c9-136">From the results, install the **MongoDB.Driver** library.</span></span> <span data-ttu-id="c17c9-137">Tím se nainstaluje balíček MongoDB.Driver a všechny závislosti.</span><span class="sxs-lookup"><span data-stu-id="c17c9-137">This installs the MongoDB.Driver package as well as all dependencies.</span></span>

4. <span data-ttu-id="c17c9-138">Spusťte aplikaci stisknutím CTRL+F5.</span><span class="sxs-lookup"><span data-stu-id="c17c9-138">Click CTRL + F5 to run the application.</span></span> <span data-ttu-id="c17c9-139">Aplikace se zobrazí v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="c17c9-139">Your app displays in your browser.</span></span> 

5. <span data-ttu-id="c17c9-140">V prohlížeči klikněte na **Vytvořit** a vytvořte v aplikaci seznamu úkolů několik nových úloh.</span><span class="sxs-lookup"><span data-stu-id="c17c9-140">Click **Create** in the browser and create a few new tasks in your task list app.</span></span>

## <a name="review-slas-in-the-azure-portal"></a><span data-ttu-id="c17c9-141">Ověření smluv SLA na webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c17c9-141">Review SLAs in the Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="c17c9-142">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="c17c9-142">Clean up resources</span></span>

<span data-ttu-id="c17c9-143">Pokud nebudete tuto aplikace nadále používat, odstraňte na základě následujícího postupu z portálu Azure Portal všechny prostředky vytvořené podle tohoto rychlého startu:</span><span class="sxs-lookup"><span data-stu-id="c17c9-143">If you're not going to continue to use this app, delete all resources created by this quickstart in the Azure portal with the following steps:</span></span>

1. <span data-ttu-id="c17c9-144">V nabídce vlevo na portálu Azure Portal klikněte na **Skupiny prostředků** a pak klikněte na název vytvořeného prostředku.</span><span class="sxs-lookup"><span data-stu-id="c17c9-144">From the left-hand menu in the Azure portal, click **Resource groups** and then click the name of the resource you created.</span></span> 
2. <span data-ttu-id="c17c9-145">Na stránce skupiny prostředků klikněte na **Odstranit**, do textového pole zadejte prostředek, který chcete odstranit, a pak klikněte na **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="c17c9-145">On your resource group page, click **Delete**, type the name of the resource to delete in the text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c17c9-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c17c9-146">Next steps</span></span>

<span data-ttu-id="c17c9-147">V tomto rychlém startu jste se seznámili s postupem vytvoření účtu databáze Azure Cosmos a spuštění webové aplikace pomocí rozhraní API pro MongoDB.</span><span class="sxs-lookup"><span data-stu-id="c17c9-147">In this quickstart, you've learned how to create an Azure Cosmos DB account and run a web app using the API for MongoDB.</span></span> <span data-ttu-id="c17c9-148">Teď můžete do účtu databáze Cosmos importovat další data.</span><span class="sxs-lookup"><span data-stu-id="c17c9-148">You can now import additional data to your Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="c17c9-149">Import dat do databáze Azure Cosmos pro rozhraní API MongoDB</span><span class="sxs-lookup"><span data-stu-id="c17c9-149">Import data into Azure Cosmos DB for the MongoDB API</span></span>](mongodb-migrate.md)

